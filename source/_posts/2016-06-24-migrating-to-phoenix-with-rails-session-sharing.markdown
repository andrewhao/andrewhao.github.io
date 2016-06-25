---
layout: post
title: "Migrating to Phoenix with Rails session sharing"
date: 2016-06-24 13:31:06 -0700
comments: true
categories:
- Elixir
- Phoenix
- Rails
---

You've heard the buzz about Phoenix and Elixir - about its fantastic concurrency features and its great developer tooling and fast-growing ecosystem. That's great! You're already plotting to build your company's next Big Thing on this new technology platform.

One of the biggest barriers to entry to launching a separate application platform is the work it takes to access and authorize users. Your current tech stack is based on Rails - and all session management and user authentication are on that platform.

You're hesitant to rewrite the whole stack from the ground up. Your stack may also not be at the scale in which you have a separate identity authentication or authorization API.

Fortunately for you, somebody's written a great Phoenix plug to share sessions between Rails and Phoenix. If you pull this off right, you'll be able to spin up your Phoenix app with new feature capabilities, all while letting Rails handle user authentication and session management. Let's get started!

### Before we begin

In this scenario, you want to build out a new, high-performant API in Phoenix that is consumed by your frontend single-page application, whose sessions are hosted on Rails. We'll call the Rails app `rails_app` and your new Phoenix app `phoenix_app`.

In the examples that follow, I'll be using the latest versions of each framework at the time of writing, Rails 4.2 and Phoenix 1.2.

We are going to take [Chris Constantin](https://github.com/cconstantin)'s excellent [`PlugRailsCookieSessionStore`](https://github.com/cconstantin/plug_rails_cookie_session_store) plug and integrate it into our Phoenix project.

### Our approach

Our approach will be to configure the two apps with the same data to store and fetch session data accordingly. These will require the identical configuration of cookie domains, encryption and signing salts, and security tokens.

### Configure the plug for Phoenix

In your `mix.exs` file, add the plug dependency:

```elixir
# mix.exs
defmodule PhoenixApp
  defp deps do
    # snip
    {:plug_rails_cookie_session_store, "~> 0.1"},
    # snip
  end
```

Now in your `web/phoenix_app/endpoint.ex` file, remove the configuration for the existing session store and add the configuration for the Rails session store.

```elixir
# lib/phoenix_app/endpoint.ex
defmodule PhoenixApp.Endpoint do
  plug Plug.Session,
    # Remove the original cookie store that comes with Phoenix, out of the box.
    # store: :cookie,
    # key: "_phoenix_app_key",
    # signing_salt: "M8emDP0h"
    store: PlugRailsCookieSessionStore,
    # Decide on a shared key for your cookie. Oftentimes, this should
    # mirror your Rails app session key
    key: "_rails_app_session",
    secure: true,
    encrypt: true,
    domain: ".#{Application.get_env(:phoenix_app, :session_cookie_auth).domain}",
    signing_salt: Application.get_env(:phoenix_app, :session_cookie_auth).signing_salt,
    encryption_salt: Application.get_env(:phoenix_app, :session_cookie_auth).encryption_salt,
    key_iterations: 1000,
    key_length: 64,
    key_digest: :sha,
    serializer: Poison
end
```

#### A quick aside on session cookies

The `domain` specifies how far down the hostname and domain the cookie key will be valid for. We set a `DOMAIN` environment variable with the value
`mydomain.com`. The goal is for these two apps to be able to be deployed at any subdomain that ends in `mydomain.com`, and still be able to share the cookie.

The `secure` flag configures the app to send a secure cookie, which only is served over SSL HTTPS connections. It is highly recommended for your site; if you haven't upgraded to SSL, you should do so now!

Our cookies are signed such that their origins are guaranteed to have been computed from our app(s). This is done for free with Rails (and Phoenix's) session libraries. The signature is derived from the `secret_key_base` and `signin_salt`.

The `encrypt` flag encrypts the contents of the cookie's value with an encryption key derived from `secret_key_base` and `encryption_salt`.

`key_iterations`, `key_length` and `key_digest` are configurations that dictate how the signing and encryption keys are derived. These are [configured to Rails' defaults](https://github.com/rails/rails/blob/4-2-stable/railties/lib/rails/application.rb) (see also: [defaults](https://github.com/rails/rails/blob/4-2-stable/activesupport/lib/active_support/key_generator.rb)).

Finally, the `Poison` serializer is the JSON encoder that stores and decodes session data.

### Configure Rails accordingly

Now, we turn our attention to Rails. Keep in mind that this approach only works with Rails cookie-based sessions, so if your session storage options are based on using database lookups or with a custom solution, you're out of luck in this article.

#### Configure the cookie store

```ruby
# config/initializer/session_store.rb

# Use cookie session storage in JSON format. Here, we scope the cookie to the root domain.
Rails.application.config.session_store :cookie_store, key: '_rails_app_session', domain: ".#{ENV['DOMAIN']}"
Rails.application.config.action_dispatch.cookies_serializer = :json

# These salts are optional, but it doesn't hurt to explicitly configure them the same between the two apps.
Rails.application.config.action_dispatch.encrypted_cookie_salt = ENV['SESSION_ENCRYPTED_COOKIE_SALT']
Rails.application.config.action_dispatch.encrypted_signed_cookie_salt = ENV['SESSION_ENCRYPTED_SIGNED_COOKIE_SALT']
```

The same configuration variables apply here. Note that if you are missing a `SESSION_ENCRYPTED_COOKIE_SALT` and `SESSION_ENCRYPTED_SIGNED_COOKIE_SALT`, you may generate a pair with any random values. (Side discussion: [some speculate](http://nipperlabs.com/rails-secretkeybase) that the `SECRET_KEY_BASE` is sufficiently long enough to not require a salt, and hence why Rails does not require you to supply one by default. In our example, we choose to supply them anyways to be explicit.)

### Implement Phoenix session code

### Heroku deployment gotchas

### Caveats and considerations

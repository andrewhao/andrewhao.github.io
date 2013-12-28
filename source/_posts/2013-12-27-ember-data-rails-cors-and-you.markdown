---
layout: post
title: Ember Data, Rails, CORS, and you!
date: 2013-12-27 16:22
comments: true
sharing: true
footer: true
categories:
- rails
- ember data
- ember
- api
- json
- cors
- ajax
- crossdomain
---

I'm starting up a new personal project involving Ember-Data and Rails
(more to come). The gist of it is that it's a pure frontend app engine
built in Yeoman and Grunt, and designed to talk to a remote API service
built on Rails.

So since it's a remote API, I've got to enable CORS, right?

## Install CORS via rack-cors

```ruby Gemfile.rb
gem "rack-cors", :require => "rack/cors"
```

```ruby config/application.rb
config.middleware.use Rack::Cors do
  allow do
    origins "*"

    resource "*",
      :headers => :any,
      :methods => [:get, :post, :put, :delete, :options, :patch]
    end

  allow do
    origins "*"
    resource "/public/*",
      :headers => :any,
      :methods => :get
  end
end
```

A very naive implementation with zero security whatsoever. Anyways.
Onward!

## Get Ember-Data DS.RESTAdapter talkin' CORS

I saw conflicting documentation on Ember-Data and CORS -- it seemed like
it should support CORS out of the box. Apparently this is not so.

In my ember app's `store.js` (or anywhere your app loads before the
application adapter is defined, do this:

```javascript store.js
$.ajaxSetup({
  crossDomain: true,
  xhrFields: {
    withCredentials: true
  }
});

Hendrix.Store = DS.Store.extend();
Hendrix.ApplicationAdapter = DS.RESTAdapter.extend({
  host: "http://localhost:3000",
})
```

[`$.ajaxSetup`](http://api.jquery.com/jQuery.ajaxSetup/), though its
usage is not recommended, is designed to set global options on the
jQuery `ajax` object. It provides some information on the options you can modify.

Why doesn't Ember support this out of the box? I think it's because they
cannot support IE, where one must use an XDR object to support CORS.

I've posted an [Ember follow-up question in the
forums](http://discuss.emberjs.com/t/ember-data-and-cors/3690) for discussion.

## Get Rails talking JSON out of its mimetype confusion.

Did you know that if you rely on the `Accepts:` header in HTTP that
Rails does not obey its ordering`*`? I was trying to figure out why my
Rails controllers were trying to render HTML instead of JSON when the
headers were:

`'Accept: application/json, text/javascript, */*; q=0.01'`

A [very long winded
discussion](https://github.com/rails/rails/issues/9940) on the Rails
project reveals that, well, nobody has it figured out yet. Most modern
browsers do obey `Accepts:` specificity, but for the sake of older
browser compatibility, the best practice for browsers is still to return
HTML when `*/*` is specified.

What does this mean for Rails developers who want to use `Accepts:`
mimetype lists? Well, we either wait for the Rails projects to support
mimetype specificity (and for older browsers to die out), or we are
encouraged to include the format explicitly in the URI.

I chose to have Ember append the `.json` suffix to the URL, thanks to
this [SO
post](http://stackoverflow.com/questions/13648807/ds-model-url-not-working-in-ember-js)

```javascript store.js
Hendrix.ApplicationAdapter = DS.RESTAdapter.extend({
  host: "http://localhost:3000",
  // Force ember-data to append the `json` suffix
  buildURL: function(record, suffix) {
    return this._super(record, suffix) + ".json";
  }
})
```

More to come how how this app works.

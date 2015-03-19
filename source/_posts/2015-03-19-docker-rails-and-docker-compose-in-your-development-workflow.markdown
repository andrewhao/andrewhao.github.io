---
layout: post
title: "Docker, Rails, and Docker Compose in your development workflow"
date: 2015-03-19 13:29
comments: true
categories:
- Docker
- Rails
---

(This post [originally appeared](http://blog.carbonfive.com/2015/03/17/docker-rails-docker-compose-together-in-your-development-workflow/) on the Carbon Five blog.)

We've been trialing the usage of Docker and [Docker Compose](https://docs.docker.com/compose/) (previously known as [fig](http://www.fig.sh)) on a Rails project here at Carbon Five. In the past, my personal experience with Docker had been that the promise of portable containerized apps was within reach, but the tooling and development workflow were still awkward - commands were complex, configuration and linking steps were complicated, and the overall learning curve was high.

My team decided to take a peek at the current landscape of Docker tools (primarily boot2docker and Docker Compose) and see how easily we could spin up a new app and integrate it into our development workflow on Mac OS X.

In the end, I've found my experience with Docker tools to be surprisingly pleasant; the tooling easily integrates with existing Rails development workflows with only a minor amount of performance overhead. Docker Compose offers a seamless way to build containers and orchestrate their dependencies, and helps lower the learning curve to build Dockerized applications. Read on to find out how we built ours.

## Introduction to docker-compose (née Fig).

Docker Compose acts as a wrapper around Docker - it links your containers together and provides syntactic sugar around some complex container linking commands.

We liked Docker Compose for its ability to coordinate and spin up your entire application and dependencies with one command. In the past, frameworks like Vagrant were easy ways to generate a standard image for your development team to use and get started on. Docker Compose offers similar benefits of decoupling the app from the host environment, but also provides the container vehicle for the app to run in all environments - that is, the container you develop in will often be the same container that you deploy to production with.

Docker (with the orchestration tooling provided by Compose) provides us the ability to:

* Upgrade versions of Ruby or Node (or whatever runtime your app requires) in production with far less infrastructure coordination than normally required.
* Reduce the number of moving parts in the deployment process. Instead of writing complex Puppet and Capistrano deployment scripts, our deployments will now center around moving images around and starting containers.
* Simplify developer onboarding by standardizing your team on the same machine images.

In this example, we will run two Docker containers - a Rails container and a MySQL container - and rely on Compose to build, link, and run them.

## Installing boot2docker, Docker, and Docker Compose.

Docker runs in a VirtualBox VM through an image called `boot2docker`. The reason we have to use `boot2docker` and VirtualBox is because the Mac OSX filesystem is not compatible with the type of filesystem required to support Docker. Hence, we must run our Docker containers within yet another virtual machine.

1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
2. Now install boot2docker and Docker Compose.

```bash
$ brew install boot2docker docker-compose
```
3. Initialize and start up boot2docker

```bash
$ boot2docker init
$ boot2docker start
```

4. Configure your Docker host to point to your boot2docker image.

```bash
$ $(boot2docker shellinit)
```

  You'll need to run this for every terminal session that invokes the `docker` or `docker-compose` command - better export this line into your `.zshrc` or `.bashrc`.

## Creating a Dockerfile

Let's start by creating a Dockerfile for this app. This specifies the base dependencies for our Rails application. We will need:

* Ruby 2.2 - for our Rails instance
* NodeJS and NPM - for installation of Karma, jshint, and other JS dependencies.
* MySQL client - for ActiveRecord tasks
* PhantomJS - for executing JS-based tests
* vim - for inspecting and editing files within our container

Create a `Dockerfile` from within your Rails app directory.

```
FROM ruby:2.2.0
RUN apt-get update -qq && apt-get install -y build-essential nodejs npm nodejs-legacy mysql-client vim
RUN npm install -g phantomjs

RUN mkdir /myapp

WORKDIR /tmp
COPY Gemfile Gemfile
COPY Gemfile.lock Gemfile.lock
RUN bundle install

ADD . /myapp
WORKDIR /myapp
RUN RAILS_ENV=production bundle exec rake assets:precompile --trace
CMD ["rails","server","-b","0.0.0.0"]
```

Let's start by breaking this up line-by-line:

```
FROM ruby:2.2.0
```
The [`FROM`](https://docs.docker.com/reference/builder/#from) directive specifies the [`library/ruby` base image from Docker Hub](https://registry.hub.docker.com/u/library/ruby/), and uses the `2.2.0` tag, which corresponds to the Ruby 2.2.0 runtime.

From here on, we are going to be executing commands that will build on this reference image.

```
RUN apt-get update -qq && apt-get install -y build-essential nodejs npm nodejs-legacy mysql-client vim
RUN npm install -g phantomjs
```

Each [`RUN`](https://docs.docker.com/reference/builder/#run) command builds up the image, installing specific application dependencies and setting up the environment. Here we install our app dependencies both from `apt` and `npm`.

### An aside on how a Docker image is built

One of the core concepts in Docker is the concept of "layers". Docker runs on operating systems that support layering filesystems such as `aufs` or `btrfs`. Changes to the filesystem can be thought of as atomic operations that can be rolled forward or backwards.

This means that Docker can effectively store its images as snapshots of each other, much like Git commits. This also has implications as to how we can build up and cache copies of the container as we go along.

The Dockerfile can be thought of as a series of rolling incremental changes to a base image - each command builds on top of the line before. This allows Docker to quickly rebuild changes to the reference image by understanding which lines have changed - and not rebuild the image from scratch each time.

Keep these concepts in mind as we talk about speeding up your Docker build in the following section.

### Fast Docker builds by caching your Gemfiles

The following steps install the required Ruby gems for Bundler, within your app container:

```
WORKDIR /tmp
COPY Gemfile Gemfile
COPY Gemfile.lock Gemfile.lock
RUN bundle install
```

Note how we sneak the gems into `/tmp`, then run the `bundle install` which downloads and installs gems into Bundler's `vendor/bundle` directory. This is a cache hack - whereas in the past we would have kept the `Gemfile`s in with the rest of the application directory in `/myapp`.

Keeping Gemfiles inline with the app would have meant that the entire `bundle install` command would have been re-run on each `docker-compose build` -- without any caching -- due to the constant change in the code in the `/myapp` directory.

By separating out the Gemfiles into their own directory, we logically separate the Gemfiles, which are far less likely to change, from the app code, which are far more likely to change. This reduces the number of times we have to wait for a clean `bundle install` to complete.

HT: [Brian Morearty: "How to skip bundle install when deploying a Rails app to Docker"](http://ilikestuffblog.com/2014/01/06/how-to-skip-bundle-install-when-deploying-a-rails-app-to-docker/)

### Adding the app

Finally, we finish our Dockerfile by adding our current app code to the working directory.

```
ADD . /myapp
WORKDIR /myapp
RUN RAILS_ENV=production bundle exec rake assets:precompile --trace
CMD ["rails","server","-b","0.0.0.0"]
```

This links the contents of the app directory on the host to the  `/myapp` directory within the container.

Note that we precompile all our assets before the container boots up - this ensures that the container is preloaded and ready to run and jives with Docker tenets that a container should be the same container that runs in development, test, and production environments.

## Setting up Docker Compose

Now that we've defined a `Dockerfile` for booting our Rails app, we turn to the Compose piece that orchestrates the linking phase between the Rails app and its dependencies - in this case, the DB.

A `docker-compose.yml` file automatically configures our application ecosystem. Here, it defines our Rails container and its db container:

```yaml
web:
  build: .
  volumes:
    - .:/myapp
  ports:
    - "3000:3000"
  links:
    - db
  env_file:
    - '.env.web'
db:
  image: library/mysql:5.6.22
  ports:
    - "13306:3306"
  env_file:
    - '.env.db'
```

A simple:

    $ docker-compose up

will spin up both the `web` and `db` instances.

One of the most powerful tools of using Docker Compose is the ability to abstract away the configuration of your server, no matter whether it is running as a development container on your computer, a test container on CI, or on your production Docker host.

The directive:

```yaml
links:
  - db
```

will add an entry for `db` into the Rails' container's `/etc/hosts`, linking the hostname to the correct container. This allows us to write our database.yml like so:

```yaml
# config/database.yml
development: &default
  host: db
```

Another important thing to note is the `volumes` configuration:

```yaml
# docker-compose.yml
volumes:
  - .:/myapp
```

This mounts the current directory `.` on the host Mac to the `/myapp` directory in the container. This allows us to make live code changes on the host filesystem and see code changes reflected in the container.

Also note that we make use of Compose's `env_file` directive, which allows us to specify environment variables to inject into the container at runtime:

```yaml
env_file:
  - '.env.web'
```

 A peek into `.env.web` shows:

```
PORT=3000
PUMA_WORKERS=1
MIN_THREADS=4
MAX_THREADS=16
SECRET_KEY_BASE=<Rails secret key>
AWS_REGION=us-west-2
# ...
```

Note that the `env_file` is powerful in that it allows us to swap out environment configurations when you deploy and run your containers. Perhaps your container needs separate configurations on dev than when on CI, or when deployed to staging or on production.

## Creating containers and booting them up.

Now it's time to assemble the container. From within the Rails app, run:

    $ docker-compose build

This downloads and builds the containers that your web app and your db will live in, linking them up. You will need to re-run the `docker-compose build` command every time you change the `Dockerfile` or `Gemfile`.

## Running your app in containers

You can bring up your Rails server and associated containers by running:

    $ docker-compose up

This is a combination of build, link, and start-services command for
each container. You should see output that indicates that both our `web` and `db` containers, as configured in the `docker-compose.yml` file, are booting up.


## Development workflow

I was pleasantly surprised to discover that developing with Docker added very little overhead to the development process. In fact, most commands that you would run for Rails simply needed to be prepended with a `docker-compose run web`.

When you want to run:           | With Docker Compose, you would run:
------------------------------  | -----------------------------
`bundle install`                | `docker-compose run web bundle install`
`rails s`                       | `docker-compose run web rails s`
`rspec spec/path/to/spec.rb`    | `docker-compose run web rspec spec/path/to/spec.rb`
`RAILS_ENV=test rake db:create` | `docker-compose run -e RAILS_ENV=test web rake db:create`
`tail -f log/development.log`   | `docker-compose run web tail -f log/development.log`

## Protips

Here are some nice development tricks I found useful when working with Docker:

* Add a `dockerhost` entry to your `/etc/hosts` file so you can visit `dockerhost` from your browser.

```bash
$ boot2docker ip
192.168.59.104
```

  Then add the IP to your `/etc/hosts`

```
192.168.59.104  dockerhost
```

  Now you can pull up your app from `dockerhost:3000`:

  ![Screenshot of your URL bar](http://i.imgur.com/5eqNJqN.png)

* Debugging containers with `docker exec`

  Sometimes you need to get inside a container to see what's _really_ happening. Perhaps you need to test whether a port is truly open, or verify that a process is truly running. This can be accomplished by grabbing the container ID with a `docker ps`, then passing that ID into the `docker exec` command:

```bash
$ docker ps
CONTAINER ID        IMAGE
301fa6331388        myrailsapp_web:latest
$ docker exec -it 301fa6331388 /bin/bash
root@301fa6331388:/myapp#
```

* Showing environment variables in a container with `docker-compose run web env`

```bash
$ docker-compose run web env
AWS_SECRET_KEY=
MAX_THREADS=16
MIN_THREADS=4
AWS_REGION=us-west-2
BUNDLE_APP_CONFIG=/usr/local/bundle
HOME=/root
#...
```

* Running an interactive debugger (like [pry](http://pryrepl.org/)) in your Docker container

  It takes a little extra work to get Docker to allow interactive terminal debugging with tools like `byebug` or `pry`. Should you desire to start your web server with debugging capabilities, you will need to use the `--service-ports` flag with the `run` command.

```bash
$ docker-compose run --service-ports web
```

  This works due to two internal implementations of `docker-compose run`:

  * `docker-compose run` creates a TTY session for your app to connect to, allowing interactive debugging. The default `docker-compose up` command does not create a TTY session.
  * The `run` command does not map ports to the Docker host by default. The `--service-ports` directive maps the container's ports to the host's ports, allowing you to visit the container from your web browser.

5. Use `slim` images when possible on production

  Oftentimes, your base image will come supplied with a `-slim` variant on Docker Hub. This usually means that the image maintainer has supplied a trimmed-down version of the container for you to use with source code and build-time files stripped and removed. You can oftentimes shave a couple hundred megabytes off your resulting image -- we did when we switched our `ruby` image from `2.2.1` to `2.2.1-slim`. This results in faster deployment times due to less network I/O from the registry to the deployment target.

## Gotchas

* Remember that your app runs in containers - so every time you do a `docker-compose run`, remember that Compose is spinning up entirely new containers for your code __but only if the containers are not up already, in which case they are linked to that (running) container__.

  This means that it's possible that you've spun up multiple instances of your app without thinking about it - for example, you may have a `web` and `db` container already up from a `docker-compose up` command, and then in a separate terminal window you run a `docker-compose run web rails c`. That spins up *another* `web` container to execute the command, but then links that container with the pre-launched `db` container.

* There is a small but noticeable performance penalty running through both the VirtualBox VM and docker. I've generally noticed waiting a few extra seconds when starting a Rails environment. My overall experience has been that the penalty has not been large enough to be painful.

## Try it out

Give this a shot and let me know how Docker has been working for you. What have your experiences been? What are ways in which you've been able to get your Docker workflow smoother? Share in the comments below.

### Coming up: integration with CI and deployment.

In upcoming blog posts, we will investigate how to use the power of Docker Compose to test and build your containers in a CI-powered workflow, push to Docker registries, and deploy to production. Stay tuned!

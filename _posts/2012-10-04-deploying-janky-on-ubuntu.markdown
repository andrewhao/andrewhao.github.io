---
author: andrewhao
comments: true
date: 2012-10-04 10:29:58
layout: post
slug: deploying-janky-on-ubuntu
title: Deploying Janky on Ubuntu
wordpress_id: 1164
categories:
- Andrew 2.0
---

[Janky](https://github.com/blog/1013-janky) is a Github-developed [Hubot](https://github.com/github/hubot) + Jenkins control interface. It's developed to be deployed on Heroku. However, what if you need it to live on an internal VM? Here's how I got it running on a Ubuntu (12.04 Precise) VM.


## Make sure you have the correct MySQL libs installed:

    sudo apt-get install mysql-server libmysqlclient-dev

## Clone janky from the [Github repository](https://github.com/github/janky)

    git clone https://github.com/github/janky.git
    cd janky

## Bootstrap your environment

The following steps are taken nearly verbatim from the "Hacking" section on the Janky README:

    script/bootstrap
    
    mysqladmin -uroot create janky_development
    mysqladmin -uroot create janky_test
    
    RACK_ENV=development bin/rake db:migrate
    RACK_ENV=test bin/rake db:migrate
    
    RACK_ENV=development bundle exec rake db:seed

## Configure Thin


Open `Gemfile` in your text editor and add:

    gem "foreman"

Then install it:

    bundle install

Then create a Procfile:

    touch Procfile

Open the Procfile in your text editor and add the following line:


    
    web: bundle exec thin start -p $PORT



Add the JANKY_* variables to your environment according to the janky README. I use zsh, so I added these as export statements in my ~/.zshenv



## Start your server




    
    bundle exec foreman start



Note that the server starts on port 5000 by default, and you can override it like so:


    PORT=8080 bundle exec foreman start


## That's it!

Let me know how that works for you!

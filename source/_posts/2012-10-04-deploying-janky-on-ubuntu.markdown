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




    
    <code>sudo apt-get install mysql-server libmysqlclient-dev
    </code>





## Clone janky from the [Github repository](https://github.com/github/janky)




    
    <code>git clone https://github.com/github/janky.git
    cd janky
    </code>





## Bootstrap your environment



The following steps are taken nearly verbatim from the "Hacking" section on the Janky README:


    
    <code>script/bootstrap
    
    mysqladmin -uroot create janky_development
    mysqladmin -uroot create janky_test
    
    RACK_ENV=development bin/rake db:migrate
    RACK_ENV=test bin/rake db:migrate
    
    RACK_ENV=development bundle exec rake db:seed
    </code>





## Configure Thin



Open `Gemfile` in your text editor and add:


    
    <code>gem "foreman"
    </code>



Then install it:


    
    <code>bundle install
    </code>



Then create a Procfile:


    
    <code>touch Procfile
    </code>



Open the Procfile in your text editor and add the following line:


    
    <code>web: bundle exec thin start -p $PORT
    </code>



Add the JANKY_* variables to your environment according to the janky README. I use zsh, so I added these as export statements in my ~/.zshenv



## Start your server




    
    <code>bundle exec foreman start
    </code>



Note that the server starts on port 5000 by default, and you can override it like so:


    
    <code>PORT=8080 bundle exec foreman start
    </code>





## That's it!



Let me know how that works for you!


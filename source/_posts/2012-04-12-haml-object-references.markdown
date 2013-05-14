---
author: andrewhao
comments: true
date: 2012-04-12 14:40:09
layout: post
slug: haml-object-references
title: HAML object references
wordpress_id: 1143
categories:
- HAML
- Rails
- Ruby
- Web
tags:
- development
- haml
- html
- rails
- ruby
- Web
---

Did you guys know that you can use the '[ ]' brackets in HAML to automatically set the id and class on a tag, kind of like Rails' [tag](http://api.rubyonrails.org/classes/ActionView/Helpers/TagHelper.html#method-i-tag) helper?




    
    # file: app/controllers/users_controller.rb
    
    def show
      @user = CrazyUser.find(15)
    end
    
    -# file: app/views/users/show.haml
    
    %div[@user, :greeting]
      %bar[290]/
      Hello!


is compiled to:

    
    <div class='greeting_crazy_user' id='greeting_crazy_user_15'>
      <bar class='fixnum' id='fixnum_581' />
      Hello!
    </div>






Keeps things nice, concise and DRY. See the [HAML documentation](http://haml-lang.com/docs/yardoc/file.HAML_REFERENCE.html#object_reference_  ).






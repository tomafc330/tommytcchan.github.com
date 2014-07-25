---
layout: post
title: "Rails 3 devise gem: sign out"
date: 2013-06-10 08:26
comments: true
categories: [rails,devise]
---
So I've been using this awesome gem for the authentication module...so far I'm loving it. Follows the convention over configuration model. I needed to log out my user and this is all I needed in my view (note the method :delete is required):
```
<%= link_to "Sign out", destroy_user_session_path, :method => :delete %>
```

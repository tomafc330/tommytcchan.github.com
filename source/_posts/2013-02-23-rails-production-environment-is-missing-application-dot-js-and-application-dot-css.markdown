---
layout: post
title: "Rails: production environment is missing application.js and application.css"
date: 2013-02-23 16:03
comments: true
categories: [rails,setup,asset-pipeline]
---

### The problem

So I'm working on deploying my rails app to production, but I was seeing some weirdness when I ran it in production mode. Firstly, the application resources pointed to application.js and application.css even though my template was not change (ie. ```  <%= stylesheet_link_tag    "application", :media => "all" %>``` and ```  <%= javascript_include_tag "application" %>```. That led to me believe something was wrong. In addition, when I ran ```rake assets:precompile```, nothing was generated to the ```public/assets``` folder. 

### What was wrong

I checked my application.rb, and the ```config.assets.enabled``` was set to ```true```. Then I realized that when I first created the app, I turned off that setting in ```production.rb```. Simply turning it back on allowed the compilation of assets and leading rails to resolve to the right path (and not application.js/css).


---
layout: post
title: "Rails: controller css isn't precompiled"
date: 2013-04-07 22:33
comments: true
categories: [rails,problems]
---
I'm in the process of converting a single page app into using rendering it from a controller, and so I was moving around some of the css around. By default Rails set the ```application.css``` to include ```//= require_tree .```. So naturally I removed it and in my ```application.html.erb``` I added the following: ```  <%= stylesheet_link_tag params[:controller] %>```

All good, right? Wrong! When I started the app in prod mode, I got an error like this: 

```
shortcuts.css isn't precompiled
```

What's going on here? Well upon some searching and reading through the asset pipeline guide, I realized that by default Rails does not compile your css under the stylesheets folder. I had to do this in ```production.rb``` to make it work: 

```
  config.assets.precompile += %w( *.css *.js )
```

---
layout: post
title: "Best practices with nginx and rails"
date: 2013-12-28 09:08
comments: true
categories: [rails,nginx,unicorn,best practices]
---
While searching on the internet for a related question, I came across a [gist](https://gist.github.com/mikhailov/3052776) that had the best practices for deploying a rails app with unicorn. Please refer to it but I thought I would comment on a few of the best practices that I observed.

1. Using the ```include``` directive.
This proved to be a great solution for when you want some of your configs checked into source control. You can control the settings without having to muck around in ```/opt/nginx/conf``` so you know what you are deploying when you restart nginx.

2. Adding the gzip headers to the static resources
The author compiled nginx with ```--with-http_gzip_static_module``` which he used later:
```
  location ~ ^/(assets|images|javascripts|stylesheets|swfs|system)/ {
    gzip_static       on;
    expires           max;
    add_header        Cache-Control public;
    add_header        Last-Modified "";
    add_header        ETag "";
 
    open_file_cache          max=1000 inactive=500s;
    open_file_cache_valid    600s;
    open_file_cache_errors   on;
    break;
  }
```

For myself I didn't want to add those headers to the ```assets``` folder, but for everything it was really sensible to do so.

3. Adding a ```open_file_cache``` for static assets. From the above, we can see that there is the ```open_file_cache``` decl. This directive sets the cache activity on. This is good when you have a busy server and so you don't have to open the file descriptor every time.




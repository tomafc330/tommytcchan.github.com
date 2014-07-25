---
layout: post
title: "Apache: SSL error: (98)Address already in use: make_sock: could not bind to address 0.0.0.0:443"
date: 2013-02-26 17:49
comments: true
categories: [apache,httpd,ssl,problems]
---
So I'm now configuring SSL with my new httpd server, and for some reason it kept giving me this error when I first started the server:

```
(98)Address already in use: make_sock: could not bind to address 0.0.0.0:443
```

But I did an ```lsof -i :443``` and I didn't see anything. 

### The solution
Turns out that I forgotten that Ubuntu's setup for httpd is a little non standard, in that they included a ports.conf file that includes a conditional which will start listening on port 443 if the ssl mod is enabled. I had also included a ```listen 443``` in my configuration, which led to the problem.

Removing the ```listen 443``` in my declaration solved the problem.
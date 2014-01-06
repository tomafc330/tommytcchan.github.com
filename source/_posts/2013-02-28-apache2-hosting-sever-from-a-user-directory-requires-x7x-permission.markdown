---
layout: post
title: "Apache2: hosting sever from a user directory requires x7x permission"
date: 2013-02-28 20:07
comments: true
categories: [apache2,httpd,setup]
---
I've been experimenting running a webapp in a a directory where I host my code, and in order for the server to read the dir, the base directory had to be x7x, such as
```
sudo chmod 774 repo
```

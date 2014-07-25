---
layout: post
title: "AngularJS: problem with factory/service methods not returning singletons!"
date: 2013-07-18 18:31
comments: true
categories: [angularjs,problems,singleton,service,factory]
---
I was looking at an issue today where for the life of me I could not figure out why my controllers are being returned different instances. I even went as far as creating a [plunkr](http://plnkr.co/edit/QLK8zX827c9SI3lSWXXj) to proof that I was not insane! 

I went back to the drawing board and looked at the code. How could they possibly be different? And then I saw it... I had something like this:

```
						angular.bootstrap(document.getElementById("content"), ["app.services"]);
```
Upon reading the angular docs a little bit more, I realized that by calling multiple initializations, we actually create new instances of services/factories, so they are actually not singletons anymore.

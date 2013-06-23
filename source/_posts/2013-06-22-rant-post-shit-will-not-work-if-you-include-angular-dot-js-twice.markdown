---
layout: post
title: "Rant post: Shit will not work if you include Angular.js twice"
date: 2013-06-22 17:28
comments: true
categories: [rails,angularjs]
---
Alright I'm going to rant more than blog for this entry. I spent the last hour trying to find out why a library I'm planning to use (jquery-upload) didn't work. I've learn that the asset pipeline is sort of a double edge sword...if you define something in the application and you define it again in your controller specific template, it will include the js file twice. Why isn't Sprockets smarter?! Also, when I include AngularJS twice, subtle things stop working. Why can't the AnglarJS guys not include the library if it detected that it's already included??! Anyways end of rant. Back to work..

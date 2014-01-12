---
layout: post
title: "Rails Wicked Wizards and lost session"
date: 2014-01-11 18:03
comments: true
categories: [rails,wizard,wicked,lost session]
---
# The problem
We've started using the Wicked gem for our wizard creations with events. Everything actually worked surprising well (using Wicked state machine). However I got stuck at an issue where if a user is logged in and starts the wizard flow, he/she would be logged out upon hitting the wizard controller.

# The solution
I took a look at the source code and couldn't find anything. After some digging around I realized it was my own fault. You see, on my home page I have a form that POSTed to the new wizard controller. However I was using the ```<form ...``` notation, which meant that the security token was not generate, which meant rails will create a new session. 

Therefore the solution was to do something like this:

```
    <%= form_tag('/events/building/build', method: 'post'...
```

---
layout: post
title: "Example: How to set up debugging with Karma (formerly Testacular) and Webstorm"
date: 2013-03-18 19:50
comments: true
categories: [setup,tutorial]
---

# Introduction

This is a test application (uses the default PhoneCat app) but it uses mocha to test the the http service. 

# Debugging

I'm using Webstorm to perform debugging. But to do this you will need to install [Karma](http://karma-runner.github.com/0.8/intro/installation.html), which is essentially Testacular 2.0.

# Set up 

To set this up, refer to the screenshots provided:

###### Step 1:
Create the Node js runner according to the screen shot.

http://screencloud.net/v/ERqK

###### Step 2:
Create the Debug configuration according to the screen shot. Note that the port will be the port that you Karma server ran on for the Node.js configuration you defined for Step 1.

http://screencloud.net/v/3sH0

###### Step 3: 
Start the Node.js run, then start the Debug configuration after you set some breakpoints.

http://screencloud.net/v/2oGA

Read more at:
https://github.com/tommytcchan/js-test-examples/tree/master/angularjs/testing/mocha

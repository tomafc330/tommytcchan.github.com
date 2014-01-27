---
layout: post
title: "ruby uninitialized constant Geokit::LatLng"
date: 2014-01-26 22:41
comments: true
categories: [rails,problems]
---

# The Problem
While developing a map specific feature for [venuespot](http://venuespot.co), I came across an error similar to the following:

```
uninitialized constant MyController::Geokit
```

At first thought I thoght that I had ```require```d the wrong names, but no that was not the case. Okay what else could it be. I checked the gems and it was in there. It was driving me nuts but I finally figured it out.

# The solution
I tested the library out by going on irb and tried to instantiate some of the classes. That worked. Then I took a look at the Gemfile.lock file and everything checked out. But little did I realize that I didn't include it in my Gemfile, which was what was needed. So including the ```geokit``` gem in there made it work. So I guess the story of the day is that explicit ```require``` of libraries requires you also to add it to the Gemfile. Either that or from what I've read, install the gem manually (gem install geokit), then adding a require 'geokit' to the top of vendor/plugins/geokit-rails/init.rb.

---
layout: post
title: "iOS and Chrome crashing and hanging"
date: 2014-02-01 22:49
comments: true
categories: [rails,problems,iOS,chrome,hanging,freeze,crashing]
---
So I spent a few hours of my Saturday trying to fix an issue that was occuring for on [venuespot](www.venuespot.co). I thought it had something to do with my code as it started to hang/freeze recently. The behavior was odd because it wasn't actually a crash (I couldn't find any crash logs under General > About Diagnostics & Usage > Diagnostic & Usage Data, which made it more difficult for me to troubleshoot.

When I turned to Google, I was told that it could be because of malformed HTML5 syntax, so I ran it through the [validator](http://validator.w3.org/). Seemed fine. At this point it was really scratching my head, and I proceeded to remove the vendor JavaScripts one by one. Still no luck.

I then proceeded to removing my own code. Still no luck.

# The Solution
I went even as far as trying to hunt down the iOS header string and rendering a message to tell the user to use Safari. Still doesn't work. So I thought, hmm, it as to be something that is added by a gem. Lo and behold, it was the ```rack-mini-profiler``` gem that was adding a bunch of js to the page. After I removed the gem, everything was fine. Yay time for a beer.


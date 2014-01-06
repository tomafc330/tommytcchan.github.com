---
layout: post
title: "Foundation 5 and Reveal plugin not working issues"
date: 2013-12-22 19:46
comments: true
categories: [foundation, js, reveal]
---
I'm working on a client project today using the new Foundation 5 version and I had a hard time with trying to get reveal to work.

I followed the docs on how one might be able to run an example. Downloaded the 5.0.2 library and extracted foundation.min.js. Now in the docs it mentions you also need to extract the individual plugins as well. Well guess what I did that and it still didn't work.

After digging around the code I realized that the authors already has it in the minifieid version -- by including the library again it somehow screws up and doesn't work. 

Removed the full ```foundation.reveal.js``` to make it work.

---
layout: post
title: "Chrome Extension and cookies not being sent when doing ajax calls"
date: 2014-07-24 18:32
comments: true
categories: [django,chrome-extension,cookie]
---
I've started writing a Chrome extension based on the book Growth hacking handbook by Jon YongFook. I've written about half a dozen or so Chrome extensions so far, but this is my first time using Django as the backend. I"ll write more posts about my learning with Django as well as the differences betweeen it and Rails (which I've been using for the past 2 years). 

The Problem
===========

I ran into an issue with my extension earlier. The extension makes an initial call to fetch a user's templates. What I wanted to do was to store the e-mail associated with the first call, then retrieve it later on. But for some reason the second call -- the one to update the template -- would fail on me. An inspection of the network reveals that it wasn't actually sending the cookie with the request, which means the session was not found.

The Solution
============

After looking at the manifest file of the extension, I realized that in my permissions I didn't have the ```localhost``` url defined, which meant that the information was not sent over. You would think that the original requests would not even be allowed to be sent, which wasn't the case. So if anyone is experiencing the same issue, I would double check the permissions variable in your ```manifest.json```

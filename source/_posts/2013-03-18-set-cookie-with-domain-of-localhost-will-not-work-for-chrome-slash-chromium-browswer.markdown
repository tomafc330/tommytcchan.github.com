---
layout: post
title: "Set cookie with domain of localhost will not work for chrome/chromium browswer"
date: 2013-03-18 12:09
comments: true
categories: [web,general]
---

So I was working on setting a cookie for one of the sites, and what I had done was something like this initially:

```
			Cookie cookie = new Cookie(
					key,
					value);
			cookie.setMaxAge(7);
			cookie.setPath("/");
			cookie.setDomain(serverDomain);
			
			response.addCookie(cookie);
```
Everything was working on localhost in Firefox and Opera. However, when I looked at the inspector for Chrome, it wasn't working! I then tried to use 127.0.0.1 instead of localhost. That worked. So I read up some more about it and apparently if you tried to set the domain to localhost on a cookie, Chrome will reject it! so the fix was to removed the ```cookie.setDomain(serverDomain);``` line and everything was great again.

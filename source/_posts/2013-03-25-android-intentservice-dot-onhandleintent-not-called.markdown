---
layout: post
title: "Android: IntentService.onHandleIntent() not called"
date: 2013-03-25 21:27
comments: true
categories: [android,problems]
---
So I'm starting my new companion app to www.codetipdaily.com and one of the first things I need to create is a service that I can use to call out to the server once a day. Naturally I gravitated towards the ```IntentSevice```, which seems to be the simplest to use. I had also overridden in my service.onStartCommand() to return ```START_STICKY```, which would restart the service should it be stopped. Once I did that though, the system stopped calling my ```onHandleIntent()``` method. What's going on?

It turned out that I had forgotten to call ```super.onStartCommand()``` before I returned the static int. So the following fixed it:

```
	@Override
	public int onStartCommand(android.content.Intent intent, int flags, int startId) {
		super.onStartCommand(intent, flags, startId);
		return START_STICKY;
	}
```

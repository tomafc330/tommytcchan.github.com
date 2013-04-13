---
layout: post
title: "Android:setting the default value of a setting with a colon"
date: 2013-03-27 20:34
comments: true
categories: [Android]
---
So I am working on my Android app and one of the basic things I needed to do was to set a default preference. To do that I was using the preference defined in the xml. However, I was getting messages like these:

```
java.lang.RuntimeException: Unable to start activity ComponentInfo{com.codetipdaily/com.codetipdaily.Preferences}: java.lang.ClassCastException: java.lang.Boolean cannot be cast to java.lang.String
```

I had no idea why, but I fixed it by setting the ```android:key``` to be a different key. I had the same key referenced as a CheckboxPreferences a few hours ago, and that seemed to have done the trick. Weird!



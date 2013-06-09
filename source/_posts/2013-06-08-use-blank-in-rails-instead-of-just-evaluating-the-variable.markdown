---
layout: post
title: "Use .blank? in rails instead of just evaluating the variable"
date: 2013-06-08 20:29
comments: true
categories: [rails,grails]
---
I've been working a lot in Rails on my spare time right now, and one of the things that I find myself often doing is testing for either an empty string, or empty array, or null.

Groovy actually does this test if you evaluate the variable, such as ```if (params)``` and it will does that check for you. However, by default, ruby does not have that mechanism, but luckily the Rails people created the ```blank?``` method for us to use.

As stated from the website:

```
An object is blank if it’s false, empty, or a whitespace string. For example, “”, “ ”, nil, [], and {} are all blank.
```

This is exactly what I need most of time. So instead of

```
if (shortcut.pc_shortcut != '') ... end
```

one can do:

```
if (!shortcut.pc_shortcut.blank?) .... end
```




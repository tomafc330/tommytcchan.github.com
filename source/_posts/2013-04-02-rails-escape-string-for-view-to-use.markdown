---
layout: post
title: "Rails: escape string for view to use."
date: 2013-04-02 20:33
comments: true
categories: [rails,view,html escape]
---
I had this issue with a Rails app where the string content has been html encoded. This was not good because I was feeding the information to view layer that contains some javascript. Therefore this was what I needed to do:

```
    @tags = Tag.all_as_str.html_safe
```

so the ```html_safe``` does the trick here for me.

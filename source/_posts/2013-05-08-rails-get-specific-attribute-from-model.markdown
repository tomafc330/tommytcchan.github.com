---
layout: post
title: "Rails: get specific attribute from model"
date: 2013-05-08 20:49
comments: true
categories: [rails,querying]
---
I was working getting just the ids from a list of models, and in Rails 3 you can actually use the ```pluck``` method for that!

```
    shortcuts = Shortcut.where(:software_id => params[:software_id]).pluck(:id)
```

This returns:

```
[58, 59, 60 ...]
```

awesome!

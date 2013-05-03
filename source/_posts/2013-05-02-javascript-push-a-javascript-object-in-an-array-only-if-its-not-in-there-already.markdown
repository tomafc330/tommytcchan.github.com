---
layout: post
title: "Javascript: push a javascript object in an array only if it's not in there already."
date: 2013-05-02 20:38
comments: true
categories: [javascript,jquery]
---
I just learned about the $.grep method today and it seem pretty cool. I had an array that I want to push objects to. However because the ```Array.push``` method doesn't care if an item is in the Array or not, I had to figure out how not to push the same object in. This is what I came up with:

```
                if (jQuery.grep($scope.recents,function (elem) {return elem.id == shortcut.id}).length == 0) {
                    $scope.recents.push(shortcut);
                }
```

Using the ```$.grep``` method, I was able to iterate through easily. One can argue I could've used the underscore library but I had jQuery only at this point so why not use this one liner instead?

---
layout: post
title: "Twitter Bootstrap: creating popovers - what a pain!"
date: 2013-04-13 17:17
comments: true
categories: [twitter-bootstrap,problems]
---
So I was troubleshooting the popover feature that bootstrap provides, and I must say that it is very buggy. I followed their tutorial but it didn't work until I put this in the beginning of my js:

```
$(document).ready(function () {
    $("*[rel=popover]")
        .popover({
            offset: 10,
            selector: '#map',
            placement: 'left'
        });
})
```

Why I needed to do this, I'm not sure but I think it's to initialize the popovers. Okay I was able to continue...however when it came time to destroy these popovers, I found that I wasn't able to do it with the their examples, ie: ```$('#element').popover('destroy')```. Tried to figure it some more, but ran out of time...at the end of the day I just used good ol' jQuery do help me do it:

```
            removeRepeatKeysPopovers: function() {
                $(".learning-popover").remove();
                $(".popover.left").remove();
            },
            removeAllPopovers: function () {
                this.removeRepeatKeysPopovers();
                $(".popover").remove();
            },
```

What a freakin pain! 

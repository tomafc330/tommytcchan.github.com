---
layout: post
title: "CSS: margin-top selector not working."
date: 2013-02-28 14:51
comments: true
categories: [css]
---
### The problem:
So I'm trying to set margin-top on a div that contains an image:

```
<div class="affiliate-logo"><a href="http://www.gamechannel.de"><img src="/logo.png"></a></div>


.affiliate-logo {
    margin-top: 15px;
    text-align: center;
}

```

However, the margin-top didn't work at all for whatever value I put in.

### The solution:

Turns out that you have to float the div and make that 100% width in order for the margin-top to kick in:

```
.affiliate-logo {
    float: left;
    margin-top: 15px;
    text-align: center;
    width: 100%;
}
```

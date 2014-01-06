---
layout: post
title: "Rails: add in extension classes to application.rb."
date: 2013-04-20 15:44
comments: true
categories: [Rails,config]
---
So I had extended the String class in Rails so I can reuse a lot of the common methods. However I was adding the ```require ext/string``` to my controller, and I needed in a few places. Therefore I moved it to application.rb to say DRY. The declaration looked a little odd though (```require 'ext/string'``` didn't work...neither did trying ```require '/lib/ext/string```)

What I had to do at the end was this: 

```
require File.expand_path('../../lib', __FILE__) + '/ext/string'
```

Looks kind of odd but it works..

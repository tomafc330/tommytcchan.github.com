---
layout: post
title: "Rails unit test: NoMethodError: undefined method authenticate for nil:NilClass"
date: 2013-08-11 11:45
comments: true
categories: [rails,unit_testing,rspec,devise]
---
I've started foraying out of the standard MiniTest framework and into rspec, since I've been writing a lot more Jasmine unit tests lately. One of the very first things I got (unfortunately) was this:

```
NoMethodError: undefined method `authenticate!' for nil:NilClass
```

Luckily, the devise docs tells you exactly what's wrong on their docs [page](https://github.com/plataformatec/devise#test-helpers) regarding test helpers. So if you are seeing the same issue, consult their docs!

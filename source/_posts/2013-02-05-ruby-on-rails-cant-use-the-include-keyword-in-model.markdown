---
layout: post
title: "Ruby on Rails: Can't use the include keyword in model"
date: 2013-02-05 21:30
comments: true
categories: 
---
I'm building the backend of my current project using rails, and one of the things I wanted to do was create a module that I can use in both my controller and model to report issues to a messaging queue, but I was stuck with on the following:

In error_mixin:
```
module ErrorMixin

  def error(msg)
    Exchange.instance.sendMsg(msg, 'error')
  end

  def info(msg)
    Exchange.instance.sendMsg(msg, 'info')
  end
end
```

In my domain class:

```
class Tip < ActiveRecord::Base

  module ErrorMixin
  
  self.get_random

  ...
```

When I tried this, it didn't work. What was the issue? It turned out that because in my module I call out to the class method get_random and that class methods can't reference module methods. 

The solution:

Simply replace the keyword include to extend instead.

---
layout: post
title: "Rails: include extra fields on returning a model object as json"
date: 2013-04-13 15:26
comments: true
categories: [Rails,howto]
---
Ever run into the issue where you are returning your domain object using ```to_json``` but you want an extra field? Well it's actually really easy to add that in there.

In your model:

```
  attr_accessor :key_stroke #note its attr_accessor not attr_accessible
```

In your rendering:
```
    render :json => matches.values.to_json(:methods => :key_stroke)
```

Of course, sometimes you don't want to return your whole object in which case you which restrict it with ```:only```



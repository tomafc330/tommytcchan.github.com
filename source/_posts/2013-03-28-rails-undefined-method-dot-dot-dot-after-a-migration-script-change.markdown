---
layout: post
title: "Rails: undefined method... after a migration script change"
date: 2013-03-28 21:56
comments: true
categories: [Rails, problem]
---
So I updated my model by changing the migration script and then doing ```rake db:drop``` and ```rake db:create```` and thought I would be okay. However, when I started my app after I changed the view layer, I got something like this: 

```
undefined method `command_name' for #<Shortcut:0x00000003c3b4e8>
```

What gives? It turned out that I also needed to modify ```schema.rb``` to create the initial table. So that is a good place to check out if you find yourself changing the migration data.

---
layout: post
title: "Rails: using link_to to include icons."
date: 2013-07-01 17:04
comments: true
categories: [rails,link_to]
---
So I was trying to make my links look pretty by adding the ```icon-*``` classes to the link. However I was using the ```link_to``` method and it was not clear reading the docs how to do that. Luckily I figured it out:

```
<%= link_to '<i class="icon-remove"></i>'.html_safe, message, {:method => :delete, :class => 'btn btn-small'}%>
```

Pretty awesome!


---
layout: post
title: "Rails: not serving assets on production."
date: 2013-03-29 10:57
comments: true
categories: [Rails,problems]
---
So I changed my environment to be production, and I had issues with serving static content. Since my app was a single page app, I needed this to work (didn't want to put nginx or apache in front of it). 

The error I was getting was this:

```
ActionController::RoutingError (No route matches [GET] "/"):
```

I did this in another project and forgot that I need to set the following parameter in ```production.rb```:

```
  config.serve_static_assets = true
```

All was good afterwards!

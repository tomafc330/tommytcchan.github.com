---
layout: post
title: "Couldn't find User with id=sign_in Devise on Rails 3"
date: 2013-06-17 08:47
comments: true
categories: [rails, devise, problems]
---
So I was working on an app that is using devise (who isn't?!) and I got this message:

```
Couldn't find User with id=sign_in
```

Turned out I had a ```resources :users``` declared as well as ```devise_for :users``` and the standard flow overridden the devise sign in flow.


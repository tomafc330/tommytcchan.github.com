---
layout: post
title: "wrong number of arguments (1 for 0)"
date: 2013-09-16 18:22
comments: true
categories: [rails,rabl,fog]
---
So I've started getting the following error message when I started a json service that reads a model which is fog enabled (with s3):
```
wrong number of arguments (1 for 0) at messages.json.rabl where line #1 raised:
```
I wasn't getting this before, so what gives? 

Solution:
=========

It turned out that I was doing this to get the asset path:

```
model.asset
```

However, that only returns the AssetUploader instance, so when it tried to render it, it gave me the error. If you want just the path to s3, you will need the ```to_s``` appended to the end.

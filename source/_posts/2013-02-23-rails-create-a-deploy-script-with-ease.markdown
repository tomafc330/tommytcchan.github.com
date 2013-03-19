---
layout: post
title: "Rails: create a deploy script with ease"
date: 2013-02-23 16:51
comments: true
categories: [rails,setup,ops]
---
I was finding myself having to deploy manually everytime I push something to github, so I've decided to write a deploy shell script to help with this task. Coming from a Java background, I found that creating a deploy script for rails is so much easier. Here is the meat of it:

In deploy.sh

```
#!/bin/bash
#first kill the server if its running
kill -9 `lsof -i :80 | awk '{if (NR!=1) {print $2}}'`
pushd ~/codetipdaily
#then pull from gh
git pull origin master
#assets
rake assets:clean
rake assets:precompile
#start server as daemon
rails server -e production -p 80 --daemon
popd
```

That was a piece of cake!

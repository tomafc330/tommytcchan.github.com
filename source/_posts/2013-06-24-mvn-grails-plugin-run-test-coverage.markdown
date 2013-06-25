---
layout: post
title: "mvn grails plugin: run test coverage"
date: 2013-06-24 17:58
comments: true
categories: [grails,mvn,coverage plugin]
---
I was stuck on how to pass the ```-coverage``` variable into a mavenized grails project. After looking from the inter web I got this:

```
mvn grails:exec -Dcommand=test-app -Dargs=-coverage
```

That did the trick. Thanks Google!

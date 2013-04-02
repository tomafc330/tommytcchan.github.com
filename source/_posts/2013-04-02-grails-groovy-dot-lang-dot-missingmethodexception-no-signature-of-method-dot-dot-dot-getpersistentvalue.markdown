---
layout: post
title: "Grails: groovy.lang.MissingMethodException: No signature of method: ....getPersistentValue()"
date: 2013-04-02 15:42
comments: true
categories: [Grails,problems]
---
So I'm writing a unit test that uses getPersistentValue in one of the controllers, and I ran into the problem where I got this error:

```
groovy.lang.MissingMethodException: No signature of method: Domain.getPersistentValue() 
```

I assumed that when I have the ```@Mock(domain)``` annotation, I would get all the available methods to me in the test. However, upon inspecting the methods using ```instance.metaClass.methods*.name```, I realized it was missing some of the methods. Therefore you will need to mock those in your tests.

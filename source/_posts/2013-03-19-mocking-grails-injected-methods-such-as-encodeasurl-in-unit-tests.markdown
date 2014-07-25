---
layout: post
title: "Mocking Grails injected methods such as encodeAsURL in unit tests."
date: 2013-03-19 17:26
comments: true
categories: [grails,groovy]
---
Today I was writing a test on a Grails service that used the encodeAsURL method. When I ran the test, I got something like the following:

```
groovy.lang.MissingMethodException: No signature of method: java.lang.String.encodeAsURL() is applicable for argument types: () values: []
```

Upon researching some more, I realized that Grails adds a whole bunch of utilit methods to the String class at runtime, such as ```"encodeAsBase64", "encodeAsHTML",
"encodeAsJSON", "encodeAsJavaScript", "encodeAsURL", "encodeAsXML"```.

So what do you do when you run into this situation in your tests? Well, do what the Grails framework does...add it to the metaClass of String! Here's the example that you need to add to the test (preferably the setup() method):

```
		String.metaClass.encodeAsURL = {
			delegate.toString()
		}
```

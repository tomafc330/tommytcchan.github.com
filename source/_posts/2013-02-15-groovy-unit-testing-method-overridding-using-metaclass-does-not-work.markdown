---
layout: post
title: "Groovy: unit testing method overridding using metaClass does not work"
date: 2013-02-15 16:04
comments: true
categories: [groovy, grails, testing]
---
So I am devoting my whole afternoon writing unit tests for one of our projects at work and I was stuck on why overridding methods using the metaClass mechanism doesn't work. Consider this code:
```
	private void registerSuccessAction(RegistrationFormCommand cmd, userId) {
			def successUrl = '/register/success'
			...
}
```
This was in my test

```
...
	controller.metaClass.registerSuccessAction = { command, userId ->
		println "called"
						}

...
```

However, I was runing into the fact that my overridding method isn't being called. What gives?

It turned out that you *need* to qualify the first command with the type if the method has a typed parameter, otherwise groovy won't override the behavior!

This works:

```
	controller.metaClass.registerSuccessAction = { RegistrationFormCommand command, userId ->
				throw new AssertionError("This should not be called.")
						}

````



---
layout: post
title: "Grails: Upgrading to 2.1.4 gives BeanDefinitionStoreException."
date: 2013-04-24 15:02
comments: true
categories: [Grails,upgrading,problems]
---
So I was upgrading my Grails installation to 2.1.4 (from 2.0.0) and one of the issues I came across was this error message when I tried to run the tests:

```
Caused by: org.springframework.beans.factory.BeanDefinitionStoreException: Invalid bean definition with name 'hibernateDatastore' defined in null: Could not resolve placeholder 'lang' 
```

Turns out that there is a problem with the evaluation of String in ```Config.groovy```

The solution is to cast the values to ```GStrings```


```
		myConf = "google.ca/${lang}/" as GString
```

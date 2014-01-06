---
layout: post
title: "Grails and Intellij: discard old dependencies"
date: 2013-02-22 10:36
comments: true
categories: [grails,intellij,setup]
---
So I'm working on a project that relies on a custom plugin, and it was listed as a dependency. However, I wanted it to reference a relative path instead. This was what was listed originally:

```
In BuildConfig.groovy
...
	plugins {
		test ":code-coverage:1.2.5"
		test ":spock:0.7"
		compile ":hibernate:2.0.0"
		compile ":quartz:1.0-RC2"
		compile ":rest:0.7"
		compile ":springcache:1.3.1"
        	compile ":mycustomplugin:1.0"
	}
}
```

In order to make it use your project relative project mycustomplugin (instead of fetching it from Archiva using maven), perform the following steps:

1. Change your BuildConfig.groovy -- remove the above decl in the plugins block.
2. Update your BuildConfig.groovy to add the following:

```
grails.plugin.location.'mycustomplugin' = "../mycustomplugin"
```
3. Run ```grails refresh-dependencies``` in Intellij

Once it refreshes then you should be able to reference the plugin in your source tree rather than from remote.



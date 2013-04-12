---
layout: post
title: "Grails: at org.apache.tools.ant.UnknownElement.execute(UnknownElement.java:291)"
date: 2013-04-12 15:10
comments: true
categories: [Grails,problems]
---
So I was having this issue with my Grails upgrade from 2.0.0 to 2.1.4: 

```
NullPointerException
at org.apache.tools.ant.UnknownElement.execute(UnknownElement.java:291)
```

I was like what's going on here? There isn't really any indications, so I had to use ```git bisect``` to see when I introduced the problem. Turns out (I don't have an explaination for this) that one of my tablib closures had an extra ```->```:

```
	def initFacebokShareBtn = { -> //note this
		...
	}
```

By having that extra notation it gave me errors. Once I removed it then it worked. Perhaps the Grails folks introduced something in 2.1.4?

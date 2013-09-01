---
layout: post
title: "AngularJS: using the $event for an a tag can cause problems."
date: 2013-09-01 12:01
comments: true
categories: [angularjs,problems]
---

So I was troubleshooting a bug on one of the projects that uses AngularJS. Consider this markup:
```
<a href="/fooPage" data-ng-click="show($event)"><span>foobar!</span></a>
```

In the js, this was what was declared:
```
	$scope.show = function (event) {
		var href = event.target.href;
		...
	};
```

Now, the interesting thing about this is that it *sometimes* works. The reason is that if you click on the link outside of the <span>, then the event.target resolves to the <a> tag. However, if you click within the span, then the event.target is the <span> tag, and there is no href there, thus throwing an error. 

The fix:

The solution is actually pretty simple. Instead of using ```event.target```, use ```event.currentTarget```. That resolves to the <a> and it will work for all cases.

Hope that helps!



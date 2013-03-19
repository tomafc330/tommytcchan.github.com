---
layout: post
title: "Caution: Testing Grails filter while overriding the request/response object in the setup()."
date: 2013-03-15 12:34
comments: true
categories: [Grails,unit-testing]
---
So I'm doing some testing on the filter recently, and I of the things I notice right away was that the previous committer had initializers all over the place:
```
void test_filters_all_with_no_utmParams_noSession_with__invalid_Cookie() {
		FilterConfig filter = findConfig('all')

		def request = new MockHttpServletRequest()
		def response = new MockHttpServletResponse()
		setUpWebCookieService(request, response)

		request.session = createSession(null, null, 'true')
		request.cookies = [new Cookie('blah')]
...
```
So I thought, why not just put of the same declarations on the setup method()? Well, it turns out that Grails dynamic closures will clobber the request/response variables. When I ran the tests with the declarations in the setup, no matter what I do I was not able to set my variable. 

After some digging around, I realized one thing:

1. The test already injects the request/response objects in your filter tests, so overriding actually causes problems, leading to my variables not being set on the correct request/response instance.

I'll let you decide what's best for yourself.

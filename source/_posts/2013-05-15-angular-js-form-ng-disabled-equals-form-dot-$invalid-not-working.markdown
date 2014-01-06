---
layout: post
title: "Angular JS - form ng-disabled='form.$invalid' not working"
date: 2013-05-15 12:58
comments: true
categories: [angualrjs,problems]
---
I was writing some validation using Angular JS, and I had something like this:
```
<button name="update" class="btn btn-primary" data-ng-disabled="form.$invalid">Edit</button>
```

Judging from the code examples I saw, that should just work. However it didn't for me, until I realized that the <form>'s id attribute had to named in order for angular to reference it:

```
<form action="/save" method="post" name="form" id="form" >
```

The problem was solved after I added the id attr to the form.



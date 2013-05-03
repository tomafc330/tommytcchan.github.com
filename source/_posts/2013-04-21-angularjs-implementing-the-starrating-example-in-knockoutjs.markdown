---
layout: post
title: "AngularJS: Implementing the starRating example in KnockoutJS."
date: 2013-04-21 11:03
comments: true
categories: [AngularJS,KnockoutJS,JavaScript]
---
I'm working on an angular js app that required the same functionality as the starRating example on KnockoutJS on this [page](http://learn.knockoutjs.com/#/?tutorial=custombindings) about Creating custom bindings in the tutorial. Bascially it was tutorial to build a star rating system. Now it was pretty neat to see how knockout does the 2 way binding, but I wanted to implemented in AngularJS to see how it looks like. 

So let's see how that looks like shall we?

In app.js:

```
//updates the star rating of the element
directives.directive('starRating', function() {
    return {
        link: function(scope, element) {
            element.addClass("starRating");
            element.bind('mouseenter', function() {
                angular.element(element).addClass('chosen');
            })
        }
    }

})
```

Some clarification here. 

In the link variable, we define a function that should be run when we do the link between the attribute and the element.

In our view:

```
              <li class="favorite-item">
                <img src="/img/question_mark_icon.png"/>
                <a href="#">Move Tool</a>
                <span star-rating/>
              </li>
```


And our css:

```
.starRating {
    width: 24px;
    height: 24px;
    background-image: url(/img/stars.png);
    display: inline-block;
    cursor: pointer;
    background-position: -24px 0;
}

.starRating.chosen {
    background-position: 0 0;
}
```

That's it! You then get something cool like this:

http://screencloud.net/v/3To7

Improvements:
- We can actually restrict this element to only span (since that was what was used on the KnockoutJS example). You can do that easily with AngularJS...I'll leave that exercise to the reader.

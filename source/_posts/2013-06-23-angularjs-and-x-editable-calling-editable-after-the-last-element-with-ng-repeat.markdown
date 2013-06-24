---
layout: post
title: "AngularJS and X-Editable - Calling .editable after the last element with ng-repeat."
date: 2013-06-23 18:39
comments: true
categories: [angularjs,x-editable,ng-repeat]
---
I had a pretty complicated scenario that I'm working on which entails the use of AngularJS and the X-Editable plugin.

The scenario.
=============

I have a page where it loads the Assets using ajax, and that collection gets rendered using an ng-repeat. Complicated to the fact is that the fields of the Assets are inline editable, which meant I need to call the ```$(.editable).editable()``` after all the elements are rendered (you can't do it on ```document.ready``` since it's loaded via ajax.

Here's the visual: http://screencloud.net/v/qbd0

The solution.
=============

Here's how I fixed it. Basically add a custom directive ```makeEditable``` which has this declaration:

```
...
        .directive('makeEditable', function() {
            return function(scope, element, attrs) {
                scope.$watch('$last',function(){
                    $('.editable').editable();
                });
            };
        });
```

and in your view, just do this:

```
...
    <div class="span4" data-toggle="modal-gallery" data-target="#modal-gallery" data-ng-repeat="file in queue" make-editable>
...
```

Now the input elements should be editable!

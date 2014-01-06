---
layout: post
title: "RSpec and Devise: not hitting controller action."
date: 2013-08-11 12:26
comments: true
categories: [rails,rspec,devise]
---
Okay so I ran into another problem. Just when I thought I got it figured out with the ```sign_in``` helper provided from the page in the previous post, a gaint boulder fell right in front of me. For whatever reason, the test method will not call into my controller method under test.

This is what I had originally:

```
	before (:each) do
		sign_in users(:organizer)
	end

	describe "GET new with no bid id" do
		it "returns not found" do
			get :new, {}, valid_session
			flash[:error].should equal('error')
		end
	end
```

Pretty straight forward one would persume right? Wrong!

Upon more digging around, I realized that Devise's helpers are overridding the session variables, and so if we do ```get :new, {}, valid_session```, the valid_session will override what was put into the session, causing issues with 'signing in'. Therefore the solution is to remove that. If you still need to populate a session, click [here](http://stackoverflow.com/a/9658682/151899) for a workaround.


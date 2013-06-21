---
layout: post
title: "Rails 3 redirect_to with flash :notice does not show up - problem."
date: 2013-06-20 22:02
comments: true
categories: [rails,problems,redirect_to]
---
I was working on my app just now and finally got rid of a stupid annoying thing -- it wouldn't update my flash messages for some reason after I do a redirect_to.

This was what I had:

```
        format.html { redirect_to venues_path, notice: 'Venue space was successfully created.' }
        format.json { render json: @venue_space, status: :created, location: @venue_space }
```

Pretty standard stuff right? But for some reason I couldn't get the flash messages working at all! I was so baffled by it but then I started tracing down the calls. 

Turns out that the ```venues_path``` redirects to the index method, and in the index method it does a redirect to the edit method, and in Rails 3 you need ```flash.keep``` in order to preserve the flash message. Doh!



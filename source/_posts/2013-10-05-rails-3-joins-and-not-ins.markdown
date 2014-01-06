---
layout: post
title: "Rails 3: joins and not ins"
date: 2013-10-05 14:26
comments: true
categories: [rails3,activerecord]
---
So I needed to write a complexish query for one of the queries for (VenueSpot.co)|[http://www.venuespot.co]. As we are only running a small instance on aws, I didn't to waste time by traversing through my related models using ```each```. I wanted to write a couple of more optimizing queries using the ```joins``` keyword and the NOT IN mechanism. At the end it looked like this:

```
@events = Event.joins(:event_durations).where("events.state = ? and is_published = ? and event_durations.start_date > ? and events.id NOT IN (?)", 'active', true, DateTime.now, @previous_venue_bid_ids).group('events.id')
```

One thing that tripped me up was that initially my ```@previous_venue_bid_ids``` was a string that had all the ids, but the api actually takes in an array of the ids, so just be really sure what you are passing is correct!

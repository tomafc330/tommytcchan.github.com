---
layout: post
title: "rails and geokit acts_as_mappable through not working"
date: 2014-01-08 17:04
comments: true
categories: [rails,problems,geokit-rails]
---
At [venuespot](http://www.venuespot.co), we are using a matching algorithm to ensure that we match relevant information with the venues. One of the things is using a distance calculator with geokit. In the examples it talks about how one can use the ```acts_as_mappable``` to ensure your model has access to the right methods.

It also talked about it is able to use the ```:through``` mechanism. Here's the example they provided:

```
class Location < ActiveRecord::Base
  belongs_to :locatable, :polymorphic => true
  acts_as_mappable
end

class Company < ActiveRecord::Base
  has_one :location, :as => :locatable  # also works for belongs_to associations
  acts_as_mappable :through => :location
end
```

However, my model isn't using polymorphic belongs_to (yet), but in theory I should be able to do something like this:

```
Company.within(distance, :origin => @somewhere)
```

However, this proved to be a problem because the db didn't like it:

```
SELECT `locations`.*, 
 (ACOS(least(1,COS(0.8595746566072073)*COS(-2.1485003092050197)*COS(RADIANS(locations.lat))*COS(RADIANS(locations.lng))+
 COS(0.8595746566072073)*SIN(-2.1485003092050197)*COS(RADIANS(locations.lat))*SIN(RADIANS(locations.lng))+
 SIN(0.8595746566072073)*SIN(RADIANS(locations.lat))))*3963.19)
 AS distance FROM `locations` WHERE ((locations.lat>48.38258088287847 AND locations.lat<50.11741911712155 AND locations.lng>-124.42871224851768 AND locations.lng<-121.7712877514823)) AND ((
 (ACOS(least(1,COS(0.8595746566072073)*COS(-2.1485003092050197)*COS(RADIANS(locations.lat))*COS(RADIANS(locations.lng))+
 COS(0.8595746566072073)*SIN(-2.1485003092050197)*COS(RADIANS(locations.lat))*SIN(RADIANS(locations.lng))+
 SIN(0.8595746566072073)*SIN(RADIANS(locations.lat))))*3963.19)
 <= 60)) //problem with locations not found
```

# The solution

It turns out that I needed to include the associated association in order for it to know what it's matching on:

```
    Venue.includes(:location).within(radius, :origin => [lat, lng]).all.each do |venue|
        ...
    end
```


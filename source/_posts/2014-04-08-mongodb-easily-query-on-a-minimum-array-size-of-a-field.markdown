---
layout: post
title: "MongoDB - easily query on a minimum array size of a field."
date: 2014-04-08 09:35
comments: true
categories: [mongodb,query]
---

In the past 2 weeks, we have been working super hard on fundraising. As a tech co-founder, my job is to not only get meetings, but also help out with the rest of the team for the data they need. One of the exercises involves storing different data from different sources, and then combining them so we can have a better view of the world. To do this I stored everything in Mongo. The specific problem I had was related to querying for at least a minimum of x elements in an array. Suppose I have a document that has an ```investors``` field, which contains an array of names. In order to query for that in Mongo 2.2+, you can easily append the ```x``` to the field itself, and set it with an ```$exists```. So, if I wanted to query for rows that have at least 10 investors, I would do something like this:

```
@coll.find({:'investors.10' => {:'$exists' => true}}).each do |row|
	puts ...
end
```

Pretty cool eh?



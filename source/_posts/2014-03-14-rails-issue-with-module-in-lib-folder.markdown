---
layout: post
title: "Rails issue with module in lib folder"
date: 2014-03-14 20:04
comments: true
categories: [rails,problems,uninitialized constant error]
---

I'm doing some refactoring work for [VenueSpot](www.venuespot.co) and I was getting this error when I started my app after I moved some of the common code into a module:

```
NameError - uninitialized constant MyController::DataSource:
```

Here was MyController:

```
class MyController
	include DataSource
	...
end
```

So far so good, what about the DataSource file?

```
# lib/datasource.rb
module DataSource
	def get_mongo_db
		db = MONGODB.db('info')
		# init
		...
		return db
	end
end
```

In my application.rb:
```
	config.autoload_paths += Dir["#{config.root}/lib/**/"]
```

Okay review the code, and I want you to know what the problem is. Okay got it? Well I didn't for about 10 mins. Guess what the issue was? 

.
..
...
....
.....

Alright I'll keep you from the suspense. 

It has to do with the file name. Since the module is ```DataSource```, the file name should be data_source.rb!!

I blame it on Friday night coding and drinking while there is beer pong going on in the background. >.<

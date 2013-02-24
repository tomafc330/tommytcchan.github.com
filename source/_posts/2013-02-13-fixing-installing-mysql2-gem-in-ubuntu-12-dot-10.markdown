---
layout: post
title: "Fixing installing mysql2 gem in Ubuntu 12.10"
date: 2013-02-13 20:59
comments: true
categories: [ruby, setup]
---

So I've decided to host my new creation on a VPS instead of using a PAAS like Heroku or Appfog. The main drawback -- I don't have control over the environment, which in the Rails case is very annoying. Apparently you can specify the ruby version but I gave up after I tried all the stops. 

Anyways I was setting up my server (using Ubuntu) and when I was doing ```bundle install``` it gave me a weird error:

```
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of
necessary libraries and/or headers.  Check the mkmf.log file for more
details.  You may need configuration options.
```

After digging into it a little bit, it was found that my server doesn't have the dev package **libmysqlclient-dev**

```
sudo apt-get install libmysqlclient-dev mysql-server
```

After doing that and then trying to install

```
gem install mysql2
```

everything worked.

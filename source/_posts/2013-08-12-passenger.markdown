---
layout: post
title: "Passenger"
date: 2013-08-12 12:52
comments: true
categories: [rails,passenger,thin,setup,problem]
---
I'm setting up our beta site for [venuespot](http://venuespot.co), and one of the golden rules is that everything should be as close to the production site as possible.

Here are the instructions for how to use passenger with nginx and rails (3.2.11) on Ubuntu 12.10. I did read the docs but I found that it was missing a few things, so hopefully this tutorial will address your problems. See the section on Troubleshooting for more solutions to the problems I encountered.

Introduction
============
First, you will want to install passenger:
```
gem install passenger
```
(Optional). Then, you would want to get the latest rvm:
```
rvm get latest
rvm reload
```

Now install nginx with the passenger mod:
```
rvmsudo passenger-install-nginx-module
(If the above fails, then you will need to update your rvm.)
```
Now get the libcurl (or whatever it tells you) library.
```
apt-get install libcurl4-openssl-dev
```

Okay, we're at the halfway point. Next we should edit the nginx config file at ```/opt/nginx/conf/nginx.conf```. Remove the original ```server``` block and add the following

```
server {
    listen 80;
    server_name localhost;
    root /yourwebapp/public; # <--- be sure to point to 'public'!
    passenger_enabled on;
}

passenger_pre_start http://localhost/;

```

Next, we need to make sure the nginx process can have 777 access to config.ru and make sure all the way up the path we have 777 as well.

```
chmod 777 /yourwebapp
chmod 777 config.ru 
```

Now create the nginx init script:

```
wget -O init-deb.sh http://library.linode.com/assets/1139-init-deb.sh
mv init-deb.sh /etc/init.d/nginx
chmod +x /etc/init.d/nginx
/usr/sbin/update-rc.d -f nginx defaults
```

Now start the server:
```
/etc/init.d/nginx start
```

There you have it folks! Navigate to localhost and you should see your site!


Troubleshooting
===============

In the event that you don't see anything, you should check your logs in ```/opt/nginx/logs``` to ensure everything was done correctly.

For example, you might see this: ```Error page:
Could not find amq-client-1.0.2 in any of the sources (Bundler::GemNotFound)```

This means your gems have not been installed. Do a bundle install in your rails dir


Or, you might see somethign like this: ```terminate called after throwing an instance of 'Passenger::FileSystemException'
  what():  Cannot stat '/yourwebapp/config.ru': Permission denied (errno=13)
```

This means you need to set your permissions correctly for the whole path (see above).


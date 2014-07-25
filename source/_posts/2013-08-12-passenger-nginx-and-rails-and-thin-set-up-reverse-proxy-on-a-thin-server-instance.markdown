---
layout: post
title: "Passenger Nginx and Rails and Thin - set up reverse proxy on a Thin server instance"
date: 2013-08-12 13:22
comments: true
categories: [rails,thin,passenger,reverse proxy,problems]
---
My previous post talked about setting passenger and rails. In this post I will talk about something different - using Nginx as a reverse proxy while having one (or many) Thin server instances.

Basically the setup is the same as the previous post, except that you want to set up the server block a little differently:

```
	upstream domain1 {
		server 127.0.0.1:3000;
		#server 127.0.0.1:3001;
	}

	server {
		listen 80;
		server_name localhost;
		root /home/tchan/repo/venuespot/public;   # <--- be sure to point to 'public'!


		location / {
			proxy_set_header  X-Real-IP  $remote_addr;
			proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header Host $http_host;
			proxy_redirect off;

			if (-f $request_filename/index.html) {
				rewrite (.*) $1/index.html break;
			}

			if (-f $request_filename.html) {
				rewrite (.*) $1.html break;
			}

			if (!-f $request_filename) {
				proxy_pass http://domain1;
			break;
			}
		}
	}
```

Why might this be a better setup than using Passenger? Well Thin uses an evented model, so for long running blocking I/O operations, it might be good to set this up if you want more fine grain control. Just be sure to check that the Thin instances are still running periodically (or make sure you catch all exceptions in your app).

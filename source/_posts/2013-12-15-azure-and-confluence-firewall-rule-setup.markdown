---
layout: post
title: "Azure and Confluence firewall rule setup problem and solution"
date: 2013-12-15 21:37
comments: true
categories: [ops]
---
Our company, VenueSpot.co has a Bizspark account from Microsoft which gives us $150/mn server credit for 3 years. Thanks Microsoft!

However I had to spend some time trying to configure the server esp with the ports so they play nicely. First off you have to enable your firewall, either doing it through ```iptables``` or ```ufc```. Initially, I did it using the latter:

```
sudo ufw enable
sudo ufw allow 8090
```
So far, so good. I then proceeded to the Azure admin interface to map the port from 8090 => 8090. Guess what that doesn't work (at least not for me.)

After some experimentation I go it to work by removing the port 80 => 80 mapping, and instead I created a new mapping that points 80 => 8090.

Coming from an AWS background, this made no sense. Surely I should be able to open any port I wanted...however that didn't work for me.

---
layout: post
title: "Apache: Setting up rewrite rules for a server in Ubuntu 12.10"
date: 2013-02-26 15:26
comments: true
categories: [apache2,httpd,Ubuntu,ajp,mod_rewrite]
--- 
### Introduction
So I was configuring a Java app today, trying to get it deployed on httpd using ajp on Ubuntu. However, there was twist to it, and it is that the /static resources would have to point to a local file system folder (checked out with SCM).

I'm going to document how this is done on Ubuntu since it was somewhat of a painful process.

### Setup
I had apache2 install via ```apt-get```. The next step was to configure the apache2 server. The first thing needed was to specify the directory where the /static path resources will look in:

```
<Directory "~/web-assets">
    Options Indexes FollowSymLinks
    AllowOverride None
    Order allow,deny
    Allow from all
</Directory>
```

The next thing to do is to set up the ajp port.

```
<VirtualHost *:80>
	...
	
	ProxyPass 			/ 			ajp://localhost:8010/
	ProxyPassReverse 	/ 			ajp://localhost:8010/

	ProxyPreserveHost On

	...
		
</VirtualHost>
```

We then need to set up the rewrite rule:

```
	(...same VirtualHost declaration...)
	RewriteRule		^/static/\d+/(.*)                              	%{DOCUMENT_ROOT}/world/$1		[L,E=STATIC_ASSET:1]
	RewriteRule		^/static/(.*)                               	%{DOCUMENT_ROOT}/world/$1		[L,E=STATIC_ASSET:1]
	...
```

It's also good to have logging:

```
	(...same VirtualHost declaration...)
	# logging
	RewriteLog "/home/tchan/logs/rewrite.log"
	RewriteLogLevel 3
	...
```

Now enable the mods (using a2enmod):
```
a2enmod proxy
a2enmod rewrite
a2enmod ssl
```

You might not need proxy or ssl but I did.

The next thing you need to do is to set up the permissions on your folder:

```
cd ~/web-assets
chmod a=rwx -R *
```

Note that if your location is somewhere else you might need to give the same permission to the parent of the folder that apache is looking it.
Also note that you can add the www-data user to the group that has the permission in your file system if you are hesitate to make that directory global.

Once you have it configured, all you need then is to configure you Java app to use the port 8010 defined above:

```
    <Connector port="8010" protocol="AJP/1.3" redirectPort="8443"/>
```

One last thing: I had to change the deploy context of the Java app from /app to / (root):
```
      <Context docBase="my-app" path="/" reloadable="true" source="org.eclipse.jst.j2ee.server:my-app"/></Host>
```


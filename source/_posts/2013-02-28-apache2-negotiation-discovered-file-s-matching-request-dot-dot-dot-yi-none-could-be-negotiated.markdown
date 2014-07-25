---
layout: post
title: "Apache2: Negotiation: discovered file(s) matching request: ...yi (None could be negotiated)"
date: 2013-02-28 20:32
comments: true
categories: [apache2,setup]
---
I received this message in my apache logs while accessing a rewrite on PHP 5.4:
```
[Thu Feb 28 20:29:44 2013] [error] [client 127.0.0.1] Negotiation: discovered file(s) matching request: /home/ad/repo/yii/backend (None could be negotiated).
```

It turned out that I needed to add an extra type defn for PHP 5.4:
```
AddType application/x-httpd-php .php
```

source: http://serverfault.com/questions/372733/apache-file-negotiation-failed

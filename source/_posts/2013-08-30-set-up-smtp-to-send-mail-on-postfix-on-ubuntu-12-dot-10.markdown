---
layout: post
title: "Set up SMTP to send mail on Postfix on Ubuntu 12.10"
date: 2013-08-30 13:52
comments: true
categories: [ubuntu,mail]
---
I needed something quick to send emails for internal use. Basically it's just a follow on [this](https://help.ubuntu.com/community/Postfix) page. However, I needed to expose the email to the different clients on our internal network. There following two extra steps were needed:

1.) In your mail config, remove this line:

``` smtpd_use_tls = no ```

2.) In the same config, update this line:

```mynetworks = 127.0.0.0/8 10.0.0.0/8```

This tells that the smtp should be allowed on the network.

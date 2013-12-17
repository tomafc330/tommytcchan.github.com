---
layout: post
title: "Rails and adding an environment to headers"
date: 2013-12-16 19:07
comments: true
categories: [rails,mail]
---
One of the more annoying things that happens when you have multiple environements is that when a mail comes in, you don't really know what is what. Therefore it's smart to have an environment shown on the email headers so you don't accidentally confuse it with the live system. To do that is relatively trivial:

```
# base_mailer.rb
class BaseMailer < ActionMailer::Base

    default from: "hello@venuespot.co" #TODO move these out
    default to: "tom@venuespot.co"

    def mail(headers={}, &block)

        unless Rails.env == 'production'
            headers[:subject].insert(0, "[#{Rails.env}] - ")
        end

        super(headers, &block)
    end
end
```

And then just extend that class instead of the the default ```ActionMailer::Base```



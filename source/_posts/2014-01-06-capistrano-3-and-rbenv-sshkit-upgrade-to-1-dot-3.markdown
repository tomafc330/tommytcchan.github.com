---
layout: post
title: "Capistrano 3 and rbenv sshkit upgrade to 1.3"
date: 2014-01-06 17:02
comments: true
categories: [capistrano,rails3,sshkit,problem]
---
I spend a bit of time today figuring out why the capistrano rbenv gem doesn't work. First it was complaining that I had an older version of the sshkit library (it was being used by an older version of the fog library). Upgraded the fob library, but it was complaining of this:
```
cap aborted!
Unable to activate capistrano-rbenv-2.0.0, because sshkit-1.3.0 conflicts with sshkit (~> 1.
2.0)
/home/tchan/repo/venuespot/Capfile:19:in `<top (required)>'
/home/tchan/.rvm/gems/ruby-2.0.0-p247@venuespot/gems/capistrano-3.0.1/lib/capistrano/applica
tion.rb:22:in `load_rakefile'
/home/tchan/.rvm/gems/ruby-2.0.0-p247@venuespot/gems/capistrano-3.0.1/lib/capistrano/applica
tion.rb:12:in `run'
```

After digging through the commit history of the [https://github.com/capistrano/rbenv/pull/21/files](gem) I realized it's a library version issue.

Uninstall the capistrano gem, then add this in your Gemfile (until the author bumps the version):

```
    gem 'capistrano-rbenv', :git => 'git@github.com:capistrano/rbenv.git', :ref => '67222bbce120323e422b051dcd167d8e2d3adbf0'
```

Hope that helps someone!

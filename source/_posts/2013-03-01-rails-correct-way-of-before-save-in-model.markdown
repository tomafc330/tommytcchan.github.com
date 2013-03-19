---
layout: post
title: "Rails: correct way of before_save in model"
date: 2013-03-01 19:09
comments: true
categories: [rails]
---
So I was reading a tutorial on the before_save and thought this was a valid syntax: 

```
  def before_save(record)

    if record.publishing
      Tip.find_all_by_publishing(true).each do |publishing_tip|
        publishing_tip.publishing = false
        publishing_tip.published = true
        publishing_tip.save
      end
    end
```

Turned out that this is the valid syntax for Rails 3.1: 

```
  before_save :update_publishing_state

  def update_publishing_state
    if self.publishing
      Tip.find_all_by_publishing(true).each do |publishing_tip|
        publishing_tip.publishing = false
        publishing_tip.published = true
        publishing_tip.save
      end
    end
  end
```

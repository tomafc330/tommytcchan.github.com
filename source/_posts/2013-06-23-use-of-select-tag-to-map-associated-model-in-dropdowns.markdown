---
layout: post
title: "Use of select_tag to map associated model in dropdowns."
date: 2013-06-23 15:33
comments: true
categories: [Rails,select_tag]
---
Here's a way to create a dropdown to save associated model in dropdown.

In _addform.html.erb:
```
          <%= select_tag(:organization_id, options_for_select(current_user.organizations.collect { |p| [p.name, p.id] }), {:include_blank => false}) %>
```

In _editform.html.erb:
```
        <%= select_tag(:organization_id, options_for_select(current_user.organizations.collect { |p| [p.name, p.id] }, :selected => @advertisement.organization.id), {:include_blank => false}) %>
```

And in the controller:

```
    @advertisement.organization = Organization.find(params[:organization_id])
```

I would like to not have two different _form templates so what I would do is probably extract that logic in the helper so we can just have one tag for both create and edit.

---
layout: post
title: "Setting up Rails with Mixpanel on both the front end and back end"
date: 2014-01-05 17:12
comments: true
categories: [rails,mixpanel,setup,howto]
---
At [venuespot](http://www.venuespot.co), we use Mixpanel. We've had an account for awhile actually, but didn't really do anything with it. I upgraded our GA to use the new GA script, and needless to say the events management isn't that great. That prompted me to look at Mixpanel again. In this article I will explain how one is able to set up events to track individual users going through the system without too much intrusive work.

# The Requirements
We need a few things we wanted to use with our app:
1. We want to track the different events that a user takes throughout our site, whether they are logged in or not.
2. We wanted to blanket track **all** events, such as going to a controller#action.
3. We don't want it to affect the performance of our system too much, which meant hooking into Sidekiq for asynchronous processing.


# The approach
If you followed the Mixpanel recommendations, they advise you to use the JavaScript library. This was a little limiting for us as we want to track when we send out emails and notifications to the users. So we took a 2 pronged approach: 
1. If the user isn't signed in and is just browsing our site, we track those events with the JS client library.
2. Once they have signed up/in though, we will use server side calls to track the events in an async manner.

# The Setup
You will need a few things:
1. Sign up with Mixpanel, and install the javascript.
2. Place the javascript in your asset pipeline folder and reference it:
```
# application.js
//= require mixpanel/mixpanel
```

# The implementation
So let's start with the part where the user is not logged in but we want to track their actions. We need to tell mixpanel that a new user on our site. Now we will be using ```localStorage``` to store our anonymous user.
```
# venuespot_tracker.js
function VenueSpot() {

    this.createOrGetUser = function () {
        //check to see if localStorage is defined, omitted for brevity's sake
        ...

        if (!localStorage.vs_user) {
            localStorage.vs_user = rand(12);
            mixpanel.identify(localStorage.vs_user);
            mixpanel.people.set({
                "$created": new Date(),
                "$last_login": new Date()
            });
        }
        return localStorage.vs_user;
    };

    this.homePageViewed = function() {
        this.createOrGetUser();
        this.track(document.location.pathname + ' page viewed'); //method omitted for brevity

        mixpanel.track_links("a", "Clicked Link", {
            referrer: document.referrer
        });
    };

}

var vsTracker = new VenueSpotTracker();
...
vsTracker.homePageViewed();
```
A couple of interesting things...I omitted some of the code, I'm sure you can fill in the missing pieces.

Another interesting thing: if there is no user defined, we have to call ```identify``` and then set some attributes to let Mixpanel we want to track this anonymous user.

We also want to track all of the links the user clicks, so we use provided ```track_links``` method.

Once that's done, we just sit back and see the results: http://screencloud.net/v/jLLi

### Sending the info to the backend
Okay, so far we've got stats on users who have not signed up. What happens when they sign up? We don't want to lose the previous history that they had, so we have to call the ```alias``` api method after they've logged in so that we can register them. For this we use the ```mixpanel-ruby``` gem.

Now you might be asking, why don't we just continue to use the JS library? Well I thought about this for a few minutes. If you did that, you would have to make sure that we send over the email down to the client. Also, any time you want to do any tracking you would have to do the same. Finally we also wanted to track the history when we send out emails. That's a backend job so that makes it really difficult.

Hopefully I've convinced you of why we want to use the backend library.

1. First thing is to include the gem in your gemfile.
```
gem 'mixpanel-ruby'
```

2. Now on the sign up flow, you have to add in a hidden field to the back end registration:
```
$(document).ready(function() {
    $('<input>').attr({
        type: 'hidden',
        id: 'mix_panel_temp_id',
        name: 'mix_panel_temp_id',
        value: venuespotTracker.createOrGetUser()
    }).appendTo('form');
});
```

We need this because we want to do the alias in the controller:

```
# create
def create
...
        MixpanelAliasWorker.perform_async(@user.email, params[:mix_panel_temp_id], @user.first_name, @user.last_name, @user.phone, @user.type)
end

Notice here we are using Sidekiq to perform our linking in an async manner. The Mixpanel api has a few methods to do with async, but I feel that this is an easier approach.

In MixpanelAliasWorker, this is what happens:
```
require 'mixpanel-ruby'

# Does alias and link up the email address
class MixpanelAliasWorker
    include Sidekiq::Worker

    def mixpanel()
        Mixpanel::Tracker.new(ENV['mix_panel'])
    end

    def perform(email, old_id, first_name, last_name, phone, user_type)
        tracker = mixpanel()
        tracker.alias email, old_id #synchrious call
        tracker.people.set(email, {
            '$first_name' => first_name,
            '$last_name' => last_name,
            '$email' => email,
            '$phone' => phone,
            'User Type' => user_type
        });
    end
end

What happens here is that when a user is registered, we alias the old temp id we created in the front end with the actual email.

Once we do that then Mixpanel will now associate all new events with that email with the previous 'anonymous' user.

3. Now that we have the user correctly linked, the next step is to track all of the events. For that we will go into the ```ApplicationController``` and add this method:
```
    def track_with_mixpanel
        if (current_user)

            if params[:id]
                instance = controller_name.classify.constantize.find(params[:id])
                id = instance.name ? instance.name : params[:id]
            end

            MixpanelTrackWorker.perform_async(current_user.email, "Page View - #{self.class.to_s}##{self.instance_variable_get('@_action_name')} #{id}", { :controller => 'test', :action => 'test'})
        end
    end
```

Let me explain what this does. We check that the current user exists, and also check if there is a params[:id]. If there is, we want to know which model that belongs to.

Rails is pretty awesome because you can actually find that out by using ```controller_name.classify```, and then make that into a constant, and finally invoke it to find the model.

I know this isn't scalable if we have tens of thousands of users, but our stage we value metrics so in the future this will have to be modified, but it gives us a good tracking for a user and all the actions they do once they are signed in.

Here's an example: http://screencloud.net/v/kLnw

So there you have it folks. Ping me if you have any issues with setting this up!

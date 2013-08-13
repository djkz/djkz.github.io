---
layout: post
title: Simple Feature Checking With Rails
---

I needed to add something like [rollout gem](https://github.com/bitlove/rollout) to my project but wanted to keep it simple and without calls to redis.
Basically what I needed is to check if particular user has access a specific feature from my app. Since devise and most other auth gems give us access to current_user helper,
we have easy access to the user model throughout the app.

Therefore having something like current_user.has_feature? :new_feature is usually sufficient for us, but we don't want to check for features directly in the User class.
To keep the feature logic separate we can add service class HasFeature and define all our feature checks inside that class:

*app/models/has_feature.rb*
{% highlight ruby %}
class HasFeature
    # return true for half of the users
    def self.new_feature(user)
        user.id % 2
    end
end
{% endhighlight %}

In this example we want to roll out a feature to half of our users, so we can do a simple split based on user id. Now we can add a way to call it from the user class:

*app/models/user.rb*
{% highlight ruby %}
def has_feature?(feature)
    HasFeature.send(feature, self)
end
{% endhighlight %}

Now we will send the feature we are looking for and the user to our service class, which will tell us whenever user has the new feature.



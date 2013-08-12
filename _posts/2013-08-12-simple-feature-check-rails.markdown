Simple Feature Checking with Rails
==================================

In User Class
{% highlight ruby %}
def has_feature?(feature)
    HasFeature.send(feature, self)
end
{% endhighlight %}

In HasFeature Class
{% highlight ruby %}
class HasFeature
    def self.test(user)
        user.id % 2
    end
end
{% endhighlight %}


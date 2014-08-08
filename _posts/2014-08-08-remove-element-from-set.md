---
layout: post
title: Remove Elements from a HashSet while iterating
tagline: Why this is not fixed in my mind...
date: 2014-08-08
disqus_identifier: 2014-08-08-1
tags: [Java]
comments: True
---

A common exception `java.util.ConcurrentModificationException` is thrown when I try to remove a element from a `HashSet` while iterating with a **For In** loop.

{% highlight java %}
for (Integer element : integerSet) {
    if(element % 2 == 0)
        integerSet.remove(element); // Throws java.util.ConcurrentModificationException
    }
}
{% endhighlight %}

The iterator pattern has a `Iterator.remove()` method that is safe.

{% highlight java %}
for (Iterator<Integer> i = integerSet.iterator(); i.hasNext();) {
    Integer element = i.next();
    if(element % 2 == 0)
        i.remove();
    }
}
{% endhighlight %}

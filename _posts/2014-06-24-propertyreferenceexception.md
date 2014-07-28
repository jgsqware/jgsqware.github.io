---
layout: post
title: "PropertyReferenceException: No property locale found"
tagline: Cannot initialize Spring Context due to incorrect JPARepository query
date: 2014-06-24
disqus_identifier: 2014-06-24-1
comments: True
tags: [Java, Spring-Data]
---

After modifying some of my JPARepository by adding new method, my tests wasn't running well anymore.
I got this error:

{% highlight java %}
    PropertyReferenceException: No property locale found for type com.mypackage.MyClass
{% endhighlight %}

After going deeper and deepr in my virtual friend *Google*, I find why it wasn't working!
And it was on my eyes from hours now...

In my repository, I've *copy/paste* a method from another repository:

{% highlight java %}
    public MyClass findByIdAndLocale(int id, Locale locale);
{% endhighlight %}

Via [Spring-Data-Jpa Query Lookup Strategies](http://docs.spring.io/spring-data/jpa/docs/1.5.2.RELEASE/reference/html/jpa.repositories.html#jpa.query-methods), you can create queries by [JPA named queries through a naming convention](http://docs.spring.io/spring-data/jpa/docs/1.5.2.RELEASE/reference/html/jpa.repositories.html#jpa.query-methods.named-queries).

So, in my previous query, Spring is trying, on the bean initialization, to create a query that fetching the **Id** and the **LOCALE** fields that doesn't exist in *MyClass.java*!

So, in my case, removing it fixed my exception, but if you need such custom request, you can used **your** query method with the annotation [@Query](http://docs.spring.io/spring-data/jpa/docs/1.5.2.RELEASE/reference/html/jpa.repositories.html#jpa.query-methods.at-query)



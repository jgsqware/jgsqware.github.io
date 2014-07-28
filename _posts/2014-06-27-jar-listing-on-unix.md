---
layout: post
title: "List the Jar file content"
tagline: "Oh god, I always forget that command ..."
date: 2014-06-27
disqus_identifier: 2014-06-27-2
comments: True
tags: [Java,Terminal]
---

Sometimes, when I deploy on a unix server, and build from the awesome friend, [Jenkins](http://jenkins-ci.org/), I fall on some error that means that some files are missing.
Do you know thet `jar` command is able to list the content directly to the console?
I always forgot about that :/!

So, as reminder for me, and for you ;), here is it:

{% highlight PowerShell %}
    jar tvd <Jar file>
{% endhighlight %}

And that's it !

Feel free to combine it with grep to search a specific file.


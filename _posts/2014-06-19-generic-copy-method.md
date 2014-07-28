
---
layout: post
title: Defining a generic copy method with inheritance
tagline: "Using &#65124;? extends T&#65125; pattern"
date: 2014-06-19
comments: True
disqus_identifier: 2014-06-19-1
tags: [Java, Generic]
---


For an intern project, I need to  develop an entity model with multiple inheritance.
So, the classic model is:

![Copy](/public/copy_model.png)


In java, I modalized it like this:

{% highlight java %}
    public abstract class Base {
        private String id;
    }

    public abstract class AccountBase extends Base {
        private String accountId;

        public void copy(AccountBase toCopy){
            this.accountId = toCopy.getAccountId();
        }
    }

    public class User extends AccountBase {
        private String name;

        @Override
        public void copy(User toCopy){ // Not compiling because it's not overriding the parent method *public void copy(AccountBase toCopy)*
            super.copy(toCopy);
            this.name = toCopy.getName();
    }

    public class FAQ extends AccountBase {
        private String question;
        private String answer;

        @Override
        public void copy(FAQ toCopy){ // Not compiling because it's not overriding the parent method *public void copy(AccountBase toCopy)*.
            super.copy(toCopy);
            this.question = toCopy.getQuestion();
            this.answer = toCopy.getAnswer();
    }
{% endhighlight %}

So, to modelise that correctly, I use an interface and generic class/method.
![Copy](/public/generic_copy_model.png)


And the java modelisation is:

{% highlight java %}

    public interface Copiable<E> {
        public void copy(E toCopy);
    }

    public abstract class Base<T extends Base> implements Copiable<T> {
        private String id;
    }

    public abstract class AccountBase<T extends AccountBase> extends Base<T> {
        private String accountId;

        public void copy(T toCopy){
            this.accountId = toCopy.getAccountId();
        }
    }

    public class User extends AccountBase<User> {
        private String name;

        @Override
        public void copy(User toCopy){
            super.copy(toCopy);
            this.name = toCopy.getName();
    }

    public class FAQ extends AccountBase<FAQ> {
        private String question;
        private String answer;

        @Override
        public void copy(FAQ toCopy){
            super.copy(toCopy);
            this.question = toCopy.getQuestion();
            this.answer = toCopy.getAnswer();
    }
{% endhighlight %}

With that code, I can override the `public void copy(T toCopy)` method with the specific object to copy.

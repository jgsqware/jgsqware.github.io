---
layout: post
title: "Error : Cannot use identity column key generation with <union-subclass> ( TABLE_PER_CLASS )"
tageline: "Id creation on TABLE_PER_CLASS inheritance"
date: 2014-07-22
category: Development
tags: [Java,Hibernate]
---

On Hibernate mapping project, I had 2 classes. A base class containing the ID Column and a child class.

**BaseEntity.java**

{% highlight java %}
@MappedSuperclass
public abstract class BaseEntity<T extends BaseEntity> implements Serializable, Copiable<T> {

    private static final long serialVersionUID = 1L;

    @Id
    @GeneratedValue
    private Integer id;
}
{% endhighlight %}

**ChildEntity.java**

{% highlight java %}
@Entity
@Inheritance(strategy= InheritanceType.TABLE_PER_CLASS)
public class ChildEntity<T extends ChildEntity> extends BaseEntity<T> implements Serializable {
    private static final long serialVersionUID = 1L;
}
{% endhighlight %}

On deployement, I had the following exception:

{% highlight java %}
Error : Cannot use identity column key generation with <union-subclass> ( TABLE_PER_CLASS )
{% endhighlight %}

This error is due to the `@Inheritance(strategy= InheritanceType.TABLE_PER_CLASS)` on the **ChildEntity.java** and the `@GeneratedValue` of the **BaseEntity.java**.

By default, `@GeneratedValue` is defined to AUTO and let the persistence unit defining the strategy of the ID creation. Apparently, with MySql, it cause that error but not with PostgreSql.

To bypass that error, you can define the `@GeneratedValue` to `@GeneratedValue(strategy= GenerationType.TABLE)`.

In this way, the Id will be created to each table created.

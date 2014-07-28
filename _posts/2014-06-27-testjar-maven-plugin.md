---
layout: post
title: How to add test dependency from another project
tagline: Test-Jar Maven Plugin
date: 2014-06-27
comments: True
tags: [Java, Maven]
---

When you do unit testing, and have many project in the architecture. Often, you need to have the same fixtures or configuration from a project to another.

But, The test package is not added in the jar, so it's not part of the dependency of the jar.

Maven have a plugin to create a .jar and put it in his repository: [test-jar](http://maven.apache.org/guides/mini/guide-attached-tests.html)

So, first add the plugin in your `pom.xml` from the project containing the test files:

{% highlight xml %}
    <project>
  <build>
    <plugins>
     <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-jar-plugin</artifactId>
       <version>2.2</version>
       <executions>
         <execution>
           <goals>
             <goal>test-jar</goal>
           </goals>
         </execution>
       </executions>
     </plugin>
    </plugins>
  </build>
</project>
{% endhighlight %}

and in the project you need that package, you write:

{% highlight xml %}
    <project>
  ...
  <dependencies>
    <dependency>
      <groupId>com.myco.app</groupId>
      <artifactId>foo</artifactId>
      <version>1.0-SNAPSHOT</version>
      <type>test-jar</type>
      <scope>test</scope>
    </dependency>
  </dependencies>
  ...
</project>
{% endhighlight %}

So, now, in your classpath you can access to those classes and configuration files.

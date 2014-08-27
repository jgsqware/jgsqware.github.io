---
layout: post
title: How to install JBoss AS on Manjaro
tagline: 
date: 2014-08-26
disqus_identifier: 2014-08-26-1657
comments: True
tags: [Manjaro,JBoss]
---

This week, I've done my first step on Arch linux distribution: [Manjaro](http://manjaro.org/).

Du to some needs at work, I had to install [JBoss AS 7](http://jbossas.jboss.org/). 
So I will describe here the few steps to achieve this:
 
###1. Install JBoss AS 7 with yaourt
  ```$ yaourt jboss``` 
    
  ![Yaourt JBoss](/public/images/terminal-yaourt.png)
 
###2. Add the JBoss bin folder to your **$PATH**

  - Edit the file *~/.bashrc* and add the line
   
    ```export JBOSS_HOME=/usr/share/jboss-as; export PATH=$JBOSS_HOME/bin:$PATH```

  - Reload your bash profile
    
    ```$ source ~/.bashrc```
    
###3. Change the user and group of the JBoss folder
 
  ```$ chown <yourUser>:<yourGroup> /usr/share/jboss-as```
 
###4. Run the server
 
  ```$ sudo standalone.sh```
    
###5. Go to the local JBoss URL
 
  [JBoss AS Main Page](http://localhost:8080/)
  
  
    
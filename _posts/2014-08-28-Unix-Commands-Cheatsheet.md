---
layout: post
title: Unix Command Cheatsheet
tagline: "“One of the keys to happiness is a bad memory.” ― Rita Mae Brown"
date: 2014-08-28
disqus_identifier: 2014-08-28-1602
comments: True
tags: [Terminal]
---

#Unix Command to not forget again!

|Unix commands                                                   | Description                                                 | 
|:---------------------------------------------------------------|:------------------------------------------------------------|
|`cd -`                                                          |Go to the last visited folder                                |
|<code>lsof -Pni &#124; grep <port></code>                       |List the applications using the `<port>` given               |
|`sudo -i -u postgres`                                           |Switch on *postgres* user                                    |

#Arch Command to not forget again!

|Arch commands                                                   | Description                                                                                        | 
|:---------------------------------------------------------------|:---------------------------------------------------------------------------------------------------|
|`pacman -S <package_name>`                                      |Install a `<package_name>` package and its dependencies via pacman                                  |
|`pacman -Ss <package_name>`                                     |Search a `<package_name>` package via pacman                                                        |
|`yaourt <package_name>`                                         |Search a `<package_name>` package on Arch repository and install it and its dependencies via yaourt |
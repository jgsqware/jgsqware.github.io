---
layout: post
title: Run Sql Script in PGAdmn III
tagline: 
date: 2014-08-28
disqus_identifier: 2014-08-28-1456
comments: True
tags: [PostgreSQL]
---

When you have a *\*.sql* file, and you want to run it from [pgAdmin III](http://www.pgadmin.org/), there is a easy way to do it.
You can run the *PSQL Console* from within pgAdmin menu *Plugins -> PSQL Console*:

![PSQL Console](/public/images/psql_console.png)

Then, go the to the path of your script and run it:

```mydb=# \i /path/to/your/awesome_script.sql```

And Voilà!
---
layout: post
title: Git - Reset the last commit already pushed to remote
tagline: When the habits kill the productivity
date: 2014-07-28
disqus_identifier: 2014-07-28-1
tags: [Git]
comments: True
---

Here is a tip to reset the last commit of a **Git** repository already pushed to remote.

I've got the problem today.
- Pull modification from *develop* to *branch*
- Conflict
    - Resolved it to fast and make mistake
    - Don't test after conflict merged files(all the pain of the world on me...)
- Pushed to remote/origin
- Cry all the day for not working commit and Collegue arguing at me :/

There is two ways to fix it.

##1. Fix the problem in a new commit.
We can fix the files not correct in the current commit and pushed it as a new one. That wasn't the way I choose because the current commit was totally messed up.

##2. Reset the current commit locally and remotely.
That's the way I've choose. I don't want to keep any file from the last commit. So the best way, is to "delete" the last commit from locally and remotely.

Here's the commands to **reset** the local repository to the **previous remote commit**

```git reset --hard HEAD~1``` (the *HEAD~1* means the commit HEAD-1)

Then force the push remotely

```git push -f```

Then the remote/local repository will be on resetted to previous commit

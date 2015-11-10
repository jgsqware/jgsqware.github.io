+++
date = "2015-11-10T14:04:24+01:00"
draft = true
title = "Docker: Tips & Tricks"

+++

# Docker: Tips & Tricks

## Removing dangling images

Dangling images are phantom images

```bash
> docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
<none>                     <none>              d06ecfad1ad3        13 days ago         889.1 MB
```

To remove it:
```bash
> docker rmi $(docker images -f "dangling=true" -q)
```

## Disable tls
https://coderwall.com/p/siqnjg/disable-tls-on-boot2docker

## Extract Dockerfile from images
http://abdelrahmanhosny.com/2015/07/11/how-to-merge-two-docker-images/

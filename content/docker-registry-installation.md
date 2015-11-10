+++
date = "2015-11-09T15:35:40+01:00"
draft = true
title = "Registry - Docker Hub, at Home, for Free!"

+++

# Registry - Docker Hub, at Home, for Free!
> Or how to have our docker registry hub locally
Due to the slow network, we need to find a way to download docker images from local network instead of Docker Hub.

This post is related to OSX docker-machine version. It's almost the same on linux box.

## Let's Run that's folk's!
Docker offers his registry as a docker image. Nice guy!

To start a registry, latest version, simply run

```bash
> docker run -p 5000:5000 --name registry registry:2
```

It will run, but will **not persist** data when the container will stopped.
So we will use [docker-compose](Docker-Compose link), Docker-compose help you to manage run of multiple container.

```yaml
# data volume container
# The container is used to store data, backup and restore
# TODO: Change to alpine images, to get a smaller one
storage:
  image: busybox
  volumes:
    - /var/lib/registry
  command: "true"

# Caching Container
# Should be use to cache registry and enhance velocity of registry
# TODO: reactivate it

# cache:
#   image: redis

# Registry container
# This container handle the registry itself.
# Is running and expose 5000 port
# It use sqlalchemy as search engine. It's needed by frontend

backend:
  image: registry:2
  ports:
    - 5000:5000
  links:
    # - cache
    - storage
  volumes_from:
    - storage
  environment:
    SETTINGS_FLAVOR: local
    STORAGE_PATH: /var/lib/registry
    SEARCH_BACKEND: sqlalchemy
    # CACHE_REDIS_HOST: cache
    # CACHE_REDIS_PORT: 6379
    # CACHE_LRU_REDIS_HOST: cache
    # CACHE_LRU_REDIS_PORT: 6379

# Frontend container
# Deploy a Frontend for registry.
# You can browse, delete,... your repository
# Update ENV_DOCKER_REGISTRY_HOST with the ip of your docker-machine: docker-machine ip <MACHINE_NAME>
# TODO: map URL to backend

frontend:
  image: konradkleine/docker-registry-frontend:v2
  ports:
    - 5080:80
  environment:
    ENV_DOCKER_REGISTRY_HOST: 192.168.99.100
    ENV_DOCKER_REGISTRY_PORT: 5000

```

> Caching through Redis is in place but not enabled actually
> I need to add the restart-always in docker-compose

Now run your docker-compose file to start your container.

```bash
# Run docker-compose up -d to start all container
# -d is for running container as daemon

> docker-compose up -d
```

Your own docker registry is now available:
- you can **pull*, **push**, **search** repository on <IP-OF-DOCKER>:5000
- you can browse the repository on <IP-OF-DOCKER>:5080

<!-- TODO insert UI screenshot-->

## Everywhere... You want it everywhere

You need to open the vm port 5080 and 5000 to put your registry on the network.

```bash
# use VBoxManage controlvm <VM-NAME> natpf1 <RULE-NAME>,tcp,<INTERFACE IP>,<VM-PORT-TO-OPEN>,,<DOCKER-PORT-TO-MAP>
# Here, we are creating a rule registry in vm registry, mapping the docker port 5000 to the vm port 5000 on every interface

> VBoxManage controlvm registry natpf1 registry,tcp,0.0.0.0,5000,,5000
> VBoxManage controlvm registry natpf1 registry,tcp,0.0.0.0,5080,,5080
```

Now, you access over the network with:
- repository: on <IP-OF-HOST>:5000
- UI: on <IP-OF-HOST>:5080

## And now... Action!

### Allow connection to insecure registry
As the registry is not secure by certificate actually (*it will be in another story*),
we need to tell docker to allow communication with this insecure registry.

It can be done by adding `--insecure-registry=<IP-OF-REGISTRY>:<PORT-OF-REGISTRY>` in the boot2docker profile.

So,

```bash
# SSH your docker-machine
# docker-machine ssh <DOCKER-MACHINE-NAME>
> docker-machine ssh registry

# Add docker-registry as resolved name in /etc/hosts
# It will be more user-friendly
> sudo vi /etc/hosts
<IP OF Registry> docker-registry
# quit vi with :wq to save file

# Open in vi the boot2docker profile.
# profile is the configuration file used by docker when it start in boot2docker
> sudo vi /var/lib/boot2docker/profile

# Add line '--insecure-registry docker-registry:5000' in **EXTRA_ARGS**
EXTRA_ARGS='
--label provider=virtualbox
--insecure-registry docker-registry:5000
'
# quit vi with :wq to save file

# Restart docker
> sudo /etc/init.d/docker restart
# Quit docker machine by CTRL-D

```

#### Errors... What?
If you try to access your insecure registry without define it as insecure-registry, you will received the following error when you will try to *push/pull*

```bash
> docker pull 192.168.99.101:5000/registry
     unable to ping registry endpoint https://192.168.99.101:5000/v0/
       v2 ping attempt failed with error: Get https://192.168.99.101:5000/v2/: tls: oversized record received with length 20527
         v1 ping attempt failed with error: Get https://192.168.99.101:5000/v1/_ping: tls: oversized record received with length 20527
```

Sometimes, docker run out of his mind when restarting
You can see it in log, with last line as
```bash
# In your docker machine. Remember, SSH things.
> tail -f /var/log/docker.log
level=fatal msg="Shutting down due to ServeAPI error: listen tcp 0.0.0.0:2376: bind: address already in use"
```

If you see that, just kill docker and start it again
```bash
> sudo killall -9 docker
> sudo /etc/init.d/docker start
```

New, it should be ok

```bash
> tail -f /var/log/docker.log
level=info msg="Daemon has completed initialization"
level=info msg="Docker daemon" commit=f4bf5c7 execdriver=native-0.2 graphdriver=aufs version=1.8.3
```

### Push image

Now, to **push** images to your own docker registry, you have to tag your image with the docker registry url in it

```bash
# docker tag <IMAGE-NAME:?tag> <REGISTRY-IP>:<PORT>/<IMAGE-NAME:?tag>
> docker tag registry docker-registry:5000/registry

```

Now you can push it

```bash
# docker push <TAG-WITH-REPO-URL>
> docker push docker-registry:5000/registry
```

### Pull image
To **pull** images from your own docker registry, you have to pull with the docker registry url in it

```bash
# docker pull <REGISTRY-IP>:<PORT>/<IMAGE-NAME:?tag>
> docker pull docker-registry:5000/registry
```

## Backup & Restore

### Backup registry data

As we have create a [Data Volume Container](http://docs.docker.com/engine/userguide/dockervolumes/), we can backup the folder where the registry save its data to a tar file easily.

We will run a new container and mount volumes from registry data container, then mount the local folder to `/backup` in the container. Then, we will compress the registry data to a tarball.

```bash
> docker run --volumes-from registry_storage_1 -v $(pwd):/backup ubuntu sh -c "cd /var/lib/registry && tar cvf /backup/registry-data.tar ."
```

You have now a file `registry-data.tar` in your local folder.

### Restore registry data

You can now restore the previously backuped registry data to a new registry data container.

So after running the docker-compose, you will have a registry data container but it will be empty. Then, you will have to uncompress the `registry-data.tar` in the corresponding directory in the registry data container.

We will run a new container and mount volumes from registry data container, and mount the local folder to `/backup`in the container. Then, we will decompress the registry data to the registry data folder.

```bash
> docker run --volumes-from registry_storage_1 -v $(pwd):/backup ubuntu sh -c "cd /var/lib/registry && tar xvf /backup/registry-data.tar"
```

New, you will can check your repository UI url, your backuped images are restored.

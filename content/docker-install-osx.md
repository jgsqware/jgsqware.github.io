+++
date = "2015-11-10T12:15:58+01:00"
draft = false
title = "Install Docker on OSX"
+++

# Let's put some Docker in your OSX

The easier way to install Docker is with the [Docker Toolbox](https://www.docker.com/docker-toolbox).

Docker toolbox install:
 - Docker Client
 - Docker Machine
 - Docker Compose
 - Docker Kitematic
 - VirtualBox

You can install Docker Toolbox via [Homebrew](Link to homebrew)

```bash
> brew install docker-toolbox
```

You can run a Docker terminal with the VM boot2docker started and environment variable configured by running **Docker Quickstart Terminal** app.

Or you can run it by yourself with **Docker Machine**


```bash
# List Docker Machine available

> docker-machine ls
NAME            ACTIVE   DRIVER       STATE     URL                         SWARM
default                  virtualbox   Running   tcp://192.168.99.101:2376

# To start a docker machine akka VM
# > docker-machine start <DOCKER-MACHINE-NAME>
> docker-machine start default

# To create a new docker machine
# docker-machine create --driver=<VM-DRIVER> <NAME-OF-VM>
# --driver=virtualbox is use in OSX to run with boot2docker
> docker-machine create --driver=virtualbox registry
```

To allow your docker-client to speak with your docker daemon inside your docker machine *you follow me, right?*, you need to configure some **environment variables** like `DOCKER_HOST=<IP-OF-VM>`.

docker-machine help you on that with:

```bash
> eval $(docker-machine env virtual-slave)

# If you want to run connect to your vm with a specific shell you can use --shell=<SHELL-NAME>

> eval $(docker-machine env virtual-slave --shell=zsh)
```

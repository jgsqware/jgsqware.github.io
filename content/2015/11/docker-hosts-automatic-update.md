+++
date = "2015-11-10T14:04:36+01:00"
title = "Docker: Update your hosts with docker-machine ips automatically"
+++

# Docker: Update your hosts with docker-machine ips automatically

I develop a small script to update your `/etc/hosts` with your docker machine ip and a related resolved name.

You can found in this [update-hosts-docker-machine.sh](https://gist.github.com/juliengarcia/9a208ca5cf6590b25150) or here is it:

```bash
#!/usr/bin/env bash
#Usage docker-machine-update-hosts.sh hosts.conf
#arg1 is a configuration file with hosts

#Remove existing lines from hosts
while read -r line; do
    [[ "$line" =~ ^#.*$ ]] && continue
  echo "Configuration: $line"
  IFS='=' read -a configuration <<< "$line"
  machine=${configuration[0]}

  IFS=',' read -a hosts <<< "${configuration[1]}"

  DOCKER_IP=$(docker-machine ip $machine)
  for host in "${hosts[@]}"; do
    echo "Adding $DOCKER_IP  $host"
    sudo bash -c "sed -i '' '/^192\.168\.99\.[[:digit:]]\{0,3\}[[:space:]]*$host/d' /etc/hosts"
    sudo bash -c "echo '$DOCKER_IP  $host' >> /etc/hosts"
  done

done < "$1"
```

You have to use it with a configuration file, `hosts.conf`.

```
# Pattern
# <docker-machine name>=<host name>,<host name>,...
registry=docker-registry
```

Then, run it with

```bash
> update-hosts-docker-machine.sh hosts.conf
```

On OSX, you need to flush the dns cache

```bash
> dscacheutil -flushcache
```

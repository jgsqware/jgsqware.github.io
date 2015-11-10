+++
date = "2015-11-10T14:04:48+01:00"
draft = true
title = "Docker & Jenkins... A good marriage"

+++

# Jenkins and Docker... A Good Marriage

We will run Jenkins and his slaves inside Docker container.

Moreover, Jenkins job will specify a docker images in which it need to be run.

With that way, we will have a fully containerized Jenkins eco-system, reliable, scalable and independent of the host environment.

# Jenkins - Master, Data and Nginx

For the Jenkins Master part, we will have 3 containers:

1. Jenkins Master

  Will handle the jenkins process.
2. Jenkins data

  JENKINS_HOME will be store inside this container.
3. Nginx

  We will use Nginx as Reverse Proxy.

```yaml
# Will handle the JENKINS_HOME data
jenkinsdata:
  build: jenkins-data

# Will handle Jenkins process
# TODO remove docker.sock, and remove 50000 port exposure
jenkinsmaster:
  build: jenkins-master
  volumes_from:
    - jenkinsdata
  ports:
    - "50000:50000"
  volumes:
    - /var/run/docker.sock:/var/run/docker.sjenkinsdata:
  build: jenkins-data

jenkinsmaster:
  build: jenkins-master
  volumes_from:
    - jenkinsdata
  ports:
    - "50000:50000"
  volumes:
    - /var/run/docker.sock:/var/run/docker.sock
    - /usr/local/bin/docker:/bin/docker

jenkinsnginx:
  build: jenkins-nginx
  ports:
    - "80:80"
  links:
    - jenkinsmaster:jenkins-maste/**ock
    - /usr/local/bin/docker:/bin/docker

# Will redirect the 80 port to Jenkins Master
jenkinsnginx:
  build: jenkins-nginx
  ports:
    - "80:80"
  links:
    - jenkinsmaster:jenkins-master
```

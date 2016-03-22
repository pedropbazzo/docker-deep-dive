# Docker Deep Dive

## Docker and Virtualization concepts

**Linux Containers**
- Containers vs Virtual Machines
- Kernel namespaces, cggroups, capabilities

**Docker Engine**
- Execution Driver: libcontainer vs LXC
- AUFS, OverlayFS, Device Mapper

**Docker Images**
- docker build | docker images | docker inspect
- Union mounts, Layering, Dockerfile

**Docker Containers**
- docker start | stop | restart

**Volumes**
- Registries
- Volumes
- Networking

## Introducing Containers

#### The Ugly Virtual Machines

We have a separate **Operational System** for each **VM**

![Virtual Machine](http://www.ntpro.nl/blog/uploads/products_vmfs_diagram.gif "Virtual Machine")

## Docker run

We will test the **docker run** command with the Jenkins official Image
```bash
$ docker run -p 8080:8080 -p 5000:5000 jenkins
```

The command above use the following: 

**-run** Parameter to indicates which port will be used

**-p** Parameter to indicates which port will be used

**-jenkins** Image name that will be used to build our container. This image will be searched locally and, if it not been found, Docker will try to search at Docker repository

A container will be created with some random name, as **high_almeida** in my example. Is difficult to work with random names, but we can change it as follow:

```bash
$ docker run --name my-jenkins -p 8080:8080 -p 5000:5000 jenkins
```

Now our container were created with the name **my-jenkins**

Note that, when we run our container, we are led to inside of the container, but we are able to run in a **daemon** mode, using the **-d** parameter as follow:

```bash
$ docker run --name my-jenkins -d -p 8080:8080 -p 5000:5000 jenkins
```

## Docker exec

After create our Jenkins container, we need to go inside of it. We muse use the **docker exec** command as follow:

```bash
$ docker exec -i -t my-jenkins /bin/bash
```

## Mounting a volume

```bash
docker run 
	--name myjenkins 
	-p 8080:8080 -p 5000:5000 
	-v /var/jenkins_home:/var/jenkins_home jenkins
```

## Useful Docker commands

Used to show all docker's containers by its **created** column

```bash
$ docker ps -a | grep "weeks ago" | awk '{print $1}'
```

Showing all containers that matches with **Exited** Status

```bash
$ docker ps -a | grep "Exited" | awk '{print $4}'
```
Note that the ```bash {print $1}``` is matched with Status Column 

Removing all **Exited** containers

```bash
$ docker rm ${docker ps -a | grep "Exited" | awk '{print $1}'}
```

Printing all containers ids

```bash
$ docker ps -qa
```

Removing all containers

```bash
$ docker rm $(docker ps -qa)
```
Note that if container is running, we can't do that. We got the following message: Conflict, You cannot remove a running container.

Removing all containers ignoring its status and ignoring the error mentioned above

```bash
$ docker rm --force $(docker ps -qa)
```

## Granting Docker Control to Non-root Users

- Docker needs to create namespaces
- Docker needs to create cgroups
- Docker needs to bind to */var/run/docker.sock*

It's a warning to adding Users to the Docker Group
- Control which user accounts are members of this group
- Regularly audit membership of the Docker Group

We can show all groups by typing

```bash
$ cat /etc/groups
```

Will show us something as

```bash
_launchservicesd:*:239:
_iconservices:*:240:
_distnote:*:241:
_nsurlsessiond:*:242:
_nsurlstoraged:*:243:
_displaypolicyd:*:244:
_astris:*:245:
com.apple.access_disabled:*:396:
com.apple.access_sessionkey:*:397:
com.apple.access_screensharing:*:398:
com.apple.access_ssh:*:399:
.
.
.
docker:x:112
```

To add an user to the Docker Group, we must type:

```bash
$ sudo gpasswd -a gama docker
```

In the previous command, we sad that we are adding a new user gama -*-a*- to the Docker Group called **docker**.

Now we can see the user added in the Docker Group

```bash
$ cat /etc/groups
.
.
.
docker:x:112:gama
```

## Docker Exceptions

**Exception 1 - Unable to connect to internet to pull images**

This exception may occur when you are trying to pull images with docker. 

For exemple, we are trying to pull the **busybox** image as follows:

```docker
$ docker run -it busybox /bin/bash

Unable to find image 'busybox:latest' locally
Pulling repository docker.io/library/busybox
```

Note that we don't have the busybox image and docker tries to pull it and you might receive the following error:

```bash
Network timed out while trying to connect to https://index.docker.io/v1/repositories/library/busybox/images. You may want to check your internet connection or if you are behind a proxy
```

If you are using **Docker Machine** on Mac, you may try this one:

```bash
$ docker-machine restart default
```

And then:

```bash
$ eval $(docker-machine env default)
```




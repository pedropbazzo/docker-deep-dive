# Docker Deep Dive

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


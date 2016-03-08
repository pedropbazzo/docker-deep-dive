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



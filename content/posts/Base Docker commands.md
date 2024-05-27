---
title: "Base Docker commands"
date: 2023-09-05T11:23:00+03:00
draft: true
---

I had to write this article for myself, because there are a lot of Docker commands I need to remembered.

```
docker ps / docker ps -a
```
This is for list running / list all (running & stopped) containers

```
docker image ls / images
```
This is for list of all images in Docker Engine on your host.

```
docker image rm <image_name> / rmi <image_name>
```
This is for delete image. 

Note, you should remove all containers made from this image first. To do it:
```
docker rm <container_name>
```

But! You can't delete running container, so first you should stop it:
```
docker stop <container_name>
```

Next,
```
docker run <image_name> / docker run <image_name> <cmd to run inside>
```
Create container from image and run this container / with some command inside this container. Flag `-d` allows to run container in detach mode (like daemon). In this case you won't see output of container in terminal. You can again attach to container's output by this command:
```
docker attach <container_name>
```

```
docker exec <container_name> <cmd to run inside>
```

Execute command inside already running container. You can also run it by executing:
```
docker exec -it <container_name> sh
```
After it you'll enter to container shell and could run any command you need.


By default there is no any port forwarding between container and host. To set it, use `-p` flag, at the left side is host port, at the right side container port:
```
docker run -p <host_port>:<app_port_inside_container> <some_image>
```

To map host folder with container folder, use flag `-v` (volume). Sequence the same: 
```
docker run -v <host/dir>:<container/dir> <some_image>
```

Some other useful commands:
Show service info about container
```
docker inspect <some_container>
```

Show logs (stdout)
```
docker logs <some_container>
```

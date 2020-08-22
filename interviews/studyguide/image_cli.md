# docker images/commands

when we build docker images it forma a new image on the top of the previous one, these base images can be used to create new containers.

*Docker registries* holds all the images [public/private]

## docker container run

*docker container run* command from the client tells to Docker daemon to run a container.
```
docker container run -it ubuntu:latest /bin/bash
```

things that happen while docker run.

- *pull image* docker checks the image locally. if not pulls from repository(docker hub), then creates an image
- *container creation* Once docker has image, it will create container
- *allocate filesystem and mount readonly layer* container creates a file system and rw layer is added to the image
- *allocate network and IP* Creates a network interface that allows the Docker container to talk to the local host
- *process we specify* runs our applications

## docker image commands

```
docker search ubuntu
docker search --filter=stars=20 ubuntu
docker pull ubuntu:latest
docker images
```

## Remove all images and containers

```
docker container  rm $(docker ps -a -q)
docker rmi $(docker images -q)
```

## docker CLI reference
https://docs.docker.com/engine/reference/commandline/docker/

## Docker container commands
```
Usage:  docker container COMMAND

Manage containers

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  inspect     Display detailed information on one or more containers
  kill        Kill one or more running containers
  logs        Fetch the logs of a container
  ls          List containers
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  prune       Remove all stopped containers
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  run         Run a command in a new container
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  wait        Block until one or more containers stop, then print their exit codes
```

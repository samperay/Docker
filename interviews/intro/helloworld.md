# Introduction Notes

Docker allows us to run our application in our containers, that takes a single command `docker container run` [post Docker 1.13 release later]

# Docker management commands
```
builder     Manage builds
config      Manage Docker configs
container   Manage containers
context     Manage contexts
engine      Manage the docker engine
image       Manage images
network     Manage networks
node        Manage Swarm nodes
plugin      Manage plugins
secret      Manage Docker secrets
service     Manage services
stack       Manage Docker stacks
swarm       Manage Swarm
system      Manage Docker
trust       Manage trust on Docker images
volume      Manage volumes
```

`docker container run` command run a process in a new container which has its own file system, its own networing and its own isolated process.

## First Docker Container

### Hello World
```
docker container run ubuntu:latest /bin/echo 'Hello World'
docker container ps
```

### Interactive Docker container
```
docker container run -it ubuntu:latest /bin/bash
```

### Detached Docker Container
This would be running in our daemon forever.
```
docker container run -d ubuntu:latest /bin/sh -c "while true; do echo Hello World;sleep 1;done"
```

### Docker Commands Cheat Sheet
`docker container ps` Query to docker daemon for information about the container it knows about
`docker container logs`
`docker container stop`

# Docker Cheat Sheet

## Container run

*docker create* creates writable container layer over specific image and prepares for running specific command.

```
docker container create -it alpine:latest bash
docker container start -ai containerid
```

```
docker container rename currentcontainer newcontainer
docker container run -it alping:latest /bin/bash
docker container delete containerid
docker container update --cpu-share 512 -m 200M containerid containername
```

## Container info

```
docker container ps
docker container logs containerid
docker container inspect --format '{{ .NetworkSettings.IPAddress }}' $(docker ps -q)
docker container events
docker container port
docker container top
docker container stats
docker container diff
```

## Container start/stop
```
docker container start
docker container stop
docker container restart
docker container pause
docker container unpause
docker container wait
docker container kill
docker container attach
```

## Images remove/create
```
docker images
docker import
docker build
docker commit
docker rmi
docker load
docker save
```

## Docker Networks
```
docker network create
docker network rm
docker newtork ls
docker network inspect
docker network connect
docker network disconnect
```

## Docker repo
```
docker login --username=username
docker logout
docker search
docker pull
docker tag dockercontainer username/image:version
docker push username/image:version
```

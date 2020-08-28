# Docker Storage

## How docker communicate with host & containers

since host machine has no access to the files inside the docker because containers are isolated from the host, to get some work around

- Most common way is to have Docker specify environment variables inside the Docker container.
- Commonly used method is a Docker data volume
- Last way of communicating with a Docker container is using *links* or *port forwarding*

# Data Volumes

Data volumes are designed to persist data, independent of the container's life cycle.

Docker therefore never automatically delete volumes when we remove a container, nor will it "garbage collect" volumes that are no longer referenced by a container.

```
docker container run -it --name ubun -v /mydata ubuntu:latest /bin/bash
[Ctrl + P + Q] without terminating shell
mkdir /mydata
touch 1 2
```

## locate volumes

check the docker volumes.

```
docker container inspect ubun
docker volume ls
```
Once you know your docker volumes, you would be able to see the same files which you created inside the docker containers

```
ls /var/lib/docker/volumes/6313ea6875005d96314c4da03d5ef984fee825398006a4c4d546038a48c37c2d/_data
```

## mount host directory within container

Create a test directory and create files on the host and we will try to mount this directory to the container

```
mkdir temptest
cd temptest/
touch file{1..3}

docker container run -it --name testing -v ${PWD}/temptest:/opt/testdir ubuntu:latest /bin/bash
root@9c2fd1216b13:/# cd opt/
root@9c2fd1216b13:/opt# cd testdir/
root@9c2fd1216b13:/opt/testdir# ls
file1  file2  file3
root@9c2fd1216b13:/opt/testdir# rm file3
cd temptest/
ls
file1  file2
```

## Sharing files

you can now share your nginx index file to the container.

```
docker container run --name mynginx -v ${PWD}/html:/usr/share/nginx/html -P -d nginx:latest
curl http://localhost:<port>
```

You can run your python scripts without installing any python using docker

```
mkdir mypythonscripts
echo "print("Hello world from python!")" > pytest.py
docker run -it --rm -v ${PWD}:/opt/ -w /opt python:latest python pytest.py
```

## Env

Occasionally, we may need to passing in environment variable to docker run with -e argument.

```
docker run -it --rm -e USERNAME=sunlnx busybox sh
echo $DOCK_VAR
```

# nginx

## run nginx official latest image random ports
```
docker container run --name my-nginx-1 -P -d nginx:latest
```
The Nginx image exposes ports 80 and 443 in the container and the -P option tells Docker to map those ports to ports on the Docker host that are randomly selected from the range between 49153 and 65535. there by we can create multiple containers without any port conflicts.

```
docker container ps
```

```
curl http://localhost:<port>
```

## run nginx official latest image specific ports
```
docker container run --name my-nginx-1 -p 8080:80 -d nginx:latest
docker container ps
```

## Content and Configuration on the Docker Host

The Nginx image uses the default Nginx configuration, so the root directory for the container is /usr/share/nginx/html.
if you have index file locally we could mount the directory to current folder and run the command

```
docker container run --name nginx-1 \
-v /tmp/nginx/html:/usr/share/nginx/html:ro -P -d nginx

docker container ps
```

Create an file in your localhost /tmp/nginx/html/index.html and curl http://localhost:<port> you would be able to find the index.html contents.


## Copy files from Docker host

instead of keeping files in the local computer. you could also copy the context and config changes from the local directory to docker container when it creates. i.e Dockerfile

```
FROM nginx:latest
COPY /tmp/nginx/html /usr/share/nginx/html
```
Build, the docker image, '.' tells the docker that the build context is the CWD. build context contains the Dockerfile and the directories to be copied.

```
docker image build -t mycustom-nginx .
docker container run --name nginx-custom -P -d mycustom-nginx
docker container ps
```

## Docker volumes
As you know we won't be able to SSH to the docker container, so if we want to edit the container files we can use shell.
```
mkdir -p ~/content/html
echo "<html> Welcome to Nginx ! </html>" > ~/content/html/index.html
```

*Dockerfile*
```
FROM nginx:latest
COPY content /usr/share/nginx/html
VOLUME /nginxvol
```

Create a new image

```
docker image build -t mycustom-nginx .
docker images
docker container run --name mycustom-nginx-1 -P -d mycustom-nginx
docker container ps
docker container inspect mycustom-nginx-1
```

From inspect command you can find out the volumes which are mounted on the localhost.

*ls /var/lib/docker/volumes/<container_vol_id>*

you could also login to the container
```
docker container run -it --volumes-from mycustom-nginx-1 debian /bin/bash
```

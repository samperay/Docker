# Docker containers/volumes for datastore

## wordpress

- One image for WordPress itself.
- previous image linked to MySQL container.
- data container image, no need to run, just to define and share volume to mysql container. later WordPress container will be linked to volume container

*Creating volume using busybox*

Docker system should maintain the consistency of the data store we're creating now in /var/lib/mysql

```
docker container run -v /var/lib/mysql --name=my_datastore -d busybox echo "My Datastore"
docker container ps -a
docker container inspect my_datastore
```
from now on, whenever anything write to will be stored in this area of local host system!
*/var/lib/docker/volumes/4581ed5b2c6f84bd892e3a23fd4730313b04e7fa012e4420b3f1656ce5cab462/_data*

```
"Mounts": [
    {
        "Type": "volume",
        "Name": "4581ed5b2c6f84bd892e3a23fd4730313b04e7fa012e4420b3f1656ce5cab462",
        "Source": "/var/lib/docker/volumes/4581ed5b2c6f84bd892e3a23fd4730313b04e7fa012e4420b3f1656ce5cab462/_data",
        "Destination": "/var/lib/mysql",
        "Driver": "local",
        "Mode": "",
        "RW": true,
        "Propagation": ""
    }
```

## MySQL Container

```
docker container run --name my_mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw --volumes-from my_datastore -d mysql:latest
docker container inspect my_mysql
```

## WordPress Container
WordPress container should have a link to our MySQL container!

we need to find out the ports of mysql which are exposed
```
docker container inspect my_mysql

"ExposedPorts": {
            "3306/tcp": {}
```

we used --link to add link to WordPress container in the form of *container-name:alias*, specified the ports: *hostPort:containerPort*

```
docker container run --link=my_mysql:mysql -p 8888:80 -d wordpress
```


Finally, we have two docker container running and one for storing the content.

```
docker container ps
```

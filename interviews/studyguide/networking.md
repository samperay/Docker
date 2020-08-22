# Docker Networking - Bridge

To see the default docker network
```
docker network ls
```

to remove unwated network which are no longer used by containers
```
docker system prune
```

If we want to get more info about a specific network, for example, the bridge:
```
docker network inspect bridge
```

When we start Docker, a default bridge network is created automatically, and newly-started containers connect to it.

In docker, a bridge network uses a software bridge which allows containers connected to the same bridge network to communicate, while providing isolation from containers which are not connected to that bridge network. The Docker bridge driver automatically installs rules in the host machine so that containers on different bridge networks cannot communicate directly with each other.

By default, the Docker server creates and configures the host system's an ethernet bridge device, docker0.

# create custom docker bridge

we can create new ones using the network create command. We can attach a new container by specifying the network name while it's being created

*default docker network*
```
docker container run -d -p 8088:80 --name nginx-server1 nginx:alpine
docker inspect nginx-server1
docker container ps
curl http://localhost:<port>
```

*custom docker network*

```
docker network create -d bridge my-bridge-network
docker container run -d -p 8788:80 --network="my-bridge-network" --name nginx-server2 nginx:alpine
docker container ps
curl http://localhost:<port>
docker inspect nginx-server2
```

Since both the containers are in different network we can't ping them
In order to test, we can login to any of the container and ping the IP
172.17.0.3 -> nginx-server1
172.18.0.2 -> nginx-server2

```
docker exec -it nginx-server2 sh

/ # ping -w 5 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
/ #
```

Create a new container attached to *my-bridge-network* and try to ping. they would be able to communicate.
172.18.0.3 -> nginx-server3

```
docker exec -it nginx-server2 sh

/ # ping -w 1 172.18.0.3
PING 172.18.0.3 (172.18.0.3): 56 data bytes
64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.069 ms
/ #
```

you can remove your network which you have created once you have your docker container no loner using the network resources

```
docker container stop nginx-server2
docker container stop nginx-server3
docker container rm nginx-server2
docker container rm nginx-server3
docker network rm my-bridge-network
```

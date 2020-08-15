add your 'public key' of your host to the machine which you are trying to dockerize. 
you need to execute from this directory only

docker build -t image_sshd .

Once build is completed, you need to run the container 

docker run -d --name containername image_sshd:latest

docker ps

verify your ip address of your container and then ssh
docker inspect containername 

passwordless SSH to the container

ssh root@<container_ip> 


#Base image of Oracle Linux 6
FROM ubuntu:18.04
MAINTAINER sunil <sunlnx@gmain.com>

# Install openssh 
RUN apt-get update && apt-get install -y openssh-server

# customize your sshd config file
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && sed -i 's/UsePAM yes/UsePAM no/' /etc/ssh/sshd_config

#create mkdir for ssh
RUN mkdir /run/sshd && mkdir /root/.ssh/ && touch /root/.ssh/authorized_keys

#copy sshkey of the current host to docker container
ADD id_rsa.pub /root/.ssh/authorized_keys

# Create password for root user
RUN echo 'root:redhat' | chpasswd

# Allow SSH port
EXPOSE 22

# ssh binary to run on container
CMD ["/usr/sbin/sshd","-D"]

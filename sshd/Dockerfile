#Base image of Oracle Linux 6
FROM oraclelinux:6
MAINTAINER sunil <sunlnx@gmain.com>

# Install openssh 
RUN yum clean all && yum install -y openssh-server shadow-utils

# Create a pair of RSA keys
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key && ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key

#create mkdir for ssh
RUN mkdir /root/.ssh/ && touch /root/.ssh/authorized_keys

#copy sshkey of the current host to docker container
ADD id_rsa.pub /root/.ssh/authorized_keys

#allow root login to the container
RUN sed -i 's/#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config

# Create password for root user
RUN echo 'root:redhat' | chpasswd

# Allow SSH port
EXPOSE 22

# ssh binary to run on container
CMD ["/usr/sbin/sshd","-D"]

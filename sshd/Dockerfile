#Base image of Oracle Linux 6
FROM oraclelinux:6
MAINTAINER sunil <sunil.amperayani@oracle.com>

# Install openssh 
RUN yum clean all && yum install -y openssh-server shadow-utils

# Create a pair of RSA keys
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key && ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key

# Create password for root user
RUN echo 'root:welcome' | chpasswd

# Allow SSH port
EXPOSE 22

# ssh binary to run on container
CMD ["/usr/sbin/sshd","-D"]

# Build from base image
FROM ubuntu:14.04
MAINTAINER sunlnx<sunlnx.gmail.com>
# update apt-get repository
RUN apt-get update && apt-get install -y openssh-server apache2 supervisor
RUN mkdir -p /var/lock/apache2 /var/run/apache2 /var/run/sshd /var/log/supervisor
RUN cp /etc/ssh/sshd_config /etc/ssh/sshd_config.original
RUN chmod a-w /etc/ssh/sshd_config.original
RUN echo 'root:password' | chpasswd
RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config
#copy config file to container
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
# Listen to port 22 and 80
EXPOSE 22 80
CMD ["/usr/bin/supervisord"]

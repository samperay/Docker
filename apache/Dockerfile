#Ubuntu 14.04 as our base image
FROM ubuntu:14.04
MAINTAINER sunlnx <sunlnx@gmail.com>
#update apt-get
RUN apt-get update
#install apache2
RUN apt-get install -y apache2
RUN mkdir /var/lock/apache2
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_LOG_DIR /var/log/apache2
ENV LANG C
# Apache process to run at background
CMD ["/usr/sbin/apache2","-D","FOREGROUND"]
EXPOSE 80

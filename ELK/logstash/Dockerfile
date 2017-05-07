FROM ubuntu:14.04
MAINTAINER sunlnx <sunlnx@gmail.com>


ENV LT_PKG_NAME logstash-1.4.2

RUN apt-get update && apt-get install -y wget openjdk-7-jre openjdk-7-jdk

RUN cd / && wget https://download.elastic.co/logstash/logstash/$LT_PKG_NAME.tar.gz && tar xzvf $LT_PKG_NAME.tar.gz && rm -f $LT_PKG_NAME.tar.gz && mv /$LT_PKG_NAME /logstash

ENV JAVA_HOME /usr/lib/jvm/java-7-openjdk-amd64
RUN export JAVA_HOME

VOLUME ["/data"]

ADD myconfig.conf /data/logstash_config.conf

WORKDIR /data

CMD ["/logstash/bin/logstash","agent","-f","/data/logstash_config.conf"]

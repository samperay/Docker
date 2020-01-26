FROM debian:latest
MAINTAINER devops@bogotobogo.com

# 1 - RUN
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -yq apt-utils
RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq htop
RUN apt-get clean

# 2 - CMD

#CMD ["htop"]
#CMD ["ls", "-l"]

# 3 - WORKDIR and ENV
WORKDIR /root
ENV DZ version1

# 4 - ADD
ADD run.sh /root/run.sh
RUN chmod +x run.sh
#CMD ["./run.sh"]

# 5 - ENTRYPOINT (vs CMD)
ENTRYPOINT ["./run.sh"]
CMD ["arg1"]

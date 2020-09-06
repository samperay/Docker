# Build docker images automatically

## Introduction

When we build images with docker, each action taken (i.e. a command executed such as apt-get install) forms a new layer on top of the previous one

## Dockerfile

*FROM*
Instruction sets the Base Image for subsequent instructions, especially easier to start by pulling an image

*MAINTAINER*
Allows us to set the Author field of the generated images.

*RUN*
The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.

The exec form makes it possible to avoid shell string munging, and to RUN commands using a base image that does not contain /bin/sh.

*Note*: To use a different shell, other than '/bin/sh', use the exec form passing in the desired shell.
e.g RUN ["/bin/bash", "-c", "echo hello"]
The exec form is parsed as a JSON array, which means that you must use double-quotes (") around words not single-quotes (').

*Note*: Unlike the shell form, the exec form does not invoke a command shell. This means that normal shell processing does not happen.
e.g RUN [ "echo", "$HOME" ] will not do variable substitution on $HOME. If you want shell processing then either use the shell form or execute a shell directly, e.g: RUN [ "sh", "-c", "echo", "$HOME" ].

*CMD*
It has 3 forms:

1. CMD ["executable","param1","param2"] (exec form, this is the preferred form)
2. CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
3. CMD command param1 param2 (shell form)

*Note*: There can only be one CMD instruction in a Dockerfile. If we list more than one CMD then only the last CMD will take effect.

The main purpose of a CMD is to provide defaults for an executing. These defaults can include an executable, or they can omit the executable, in which case we must specify an ENTRYPOINT instruction as well.

*WORDDIR AND ENV* The WORKDIR instruction sets the working directory for any RUN, CMD and ENTRYPOINT instructions that follow it in the Dockerfile. It can be used multiple times in the one Dockerfile

The WORKDIR instruction can resolve environment variables previously set using ENV.

The ENV instruction sets the environment variable <key> to the value <value>. This value will be passed to all future RUN instructions. The environment variables set using ENV will persist when a container is run from the resulting image.

*ADD*
copies new files, directories or remote file URLs from <src> and adds them to the filesystem of the container at the path <dest>.

*ENTRYPOINT*
It has 2 forms:

1. ENTRYPOINT ["executable", "param1", "param2"] (preferred exec form - json array form)
2. ENTRYPOINT command param1 param2 (shell form)

An ENTRYPOINT allows us to configure a container that will run as an executable.

Any command line arguments passed to docker run <image> will be appended to the entrypoint command, and will override all elements specified using CMD. For example, docker run <image> bash will add the command argument bash to the end of the entrypoint.



*dockersamplefile* this file is complet, try to start from RUN and comment the other code and start to build image and try to run the containers, you can understand better as you try

```
FROM debian:latest
MAINTAINER sunlnx@gmail.com

# 1 - RUN
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -yq apt-utils
RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq htop
RUN apt-get clean

# 2 - CMD
CMD ["htop"]
CMD ["ls", "-l"]

# 3 - WORKDIR and ENV
WORKDIR /root
ENV DZ version1

# 4 - ADD
ADD run.sh /root/run.sh
CMD ["./run.sh"]

# 5 - ENTRYPOINT (vs CMD)
ENTRYPOINT ["./run.sh"]
CMD ["arg1"]
```

## RUN

```
# 1 - RUN
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -yq apt-utils
RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq htop
RUN apt-get clean
```

*docker image build* The path to the source repository defines where to find the context of the build. The build is run by the Docker daemon, not by the CLI, so the whole context must be transferred to the daemon. The Docker CLI reports "Sending build context to Docker daemon" when the context is sent to the daemon.

Note: The docker image build command will use whatever directory contains the Dockerfile as the build context (including all of its subdirectories)

```
docker image build -t name:v1 .
docker images
```

you can rebuild images now and it will be more faster as it caches the images.
docker image build -t name:v2 . -> this is much faster

since the tags are been there, you need to provide that when you are using it all the time, if there are no tags which are created, then it would take the *latest* tag by default.

```
docker image build -t dockerfiletest .
docker container run -it --rm dockerfiletest /bin/bash
```

## CMD

```
# 2 - CMD
CMD ["htop"]
CMD ["ls -l"]
```

first, build the image with your own tags
```
docker image build -t dockerfiletest .
```

when you run the command *docker container run -it --rm dockerfiletest /bin/bash* it would enter to shell and then you would try to execute *htop*.

In this case when you try to *docker container run -it --rm dockerfiletest* it would open *htop* without our intervention as this is the default environment.

So, even though we issue docker run without passing in any command, we have htop running automatically when the container is created.

we will now comment *htop* and only list files by creating a new docker image.

*docker container run -it --rm dockerfiletest* would only list the dirs/files.

## WORKDIR && ENV

```
# 3 - WORKDIR and ENV
WORKDIR /root
ENV DZ version1
```

first, build the image with your own tags
```
docker image build -t dockerfiletest .
docker images
docker container run -it --rm dockerfiletest /bin/bash
```

## ADD

```
ADD run.sh /root/run.sh
RUN chmod +x run.sh
CMD ["./run.sh"]
```

first, build the image with your own tags
```
docker image build -t dockerfiletest .
docker images
```

container runs even when there are no arguments are being passed

```
docker container run -it --rm dockerfiletest
```

you can also pass arguments while running *docker run*

```
docker container run -it --rm dockerfiletest ./run.sh first second
```

## ENTRYPOINT

```
ENTRYPOINT ["./run.sh"]
CMD ["arg1"]
```

first, build the image with your own tags
```
docker image build -t dockerfiletest .
docker images
```
container can run without any argument, it works just as an executable.
```
docker container run -it --rm dockerfiletest
```
or
```
docker container run -it --rm dockerfiletest /bin/bash
```

## ENTRYPOINT Vs CMD
Containers are meant to run a task or a process. A container lives as long as a process within it is running. If an application in a container crashes, container exits.

who defines which process should be running inside a container?
CMD tells the Docker which program shoud be run when the container starts.

Difference between the CMD and ENTRYPOINT with related to the supplied to the "docker run" command. While the CMD will be completely over-written by the supplied command (or args), for the ENTRYPOINT, the supplied command will be appended to it.

e.g

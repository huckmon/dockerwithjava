# Introduction

This is a container I made initially intended for hosting minecraft modpack servers quickly and switch between different modpacks while containerised. Also a good excuse for me to learn how to build containers.

I am not an expert at docker so please take most of this documentation with a grain of salt. Any feedback on improving this is appreciated.

Quick notes
- The provided container images don't expose ports by default. This guide somewhat covers how to do this with the docker command and compose files
- There is no latest tag, you'll need to select the tag that best suits your use case. See Versions sections for a little bit more info
- All container images are built with nano installed should you need to edit files

# Docker quick start

`docker run -d -it -expose "25565" -p 25565:25565 -v ./modpackserverfolder:/home/modpackserverfolder huckmon/ubuntuwithjava:ubuntuwithjava8`

Very basic docker command to run the container. Refer to the docker-compose section for more indepth stuff.

# Docker-compose

## Quick Start

Example compose file 

```
version: "3.8"

services:
  ubuntuwithjava:
    image: huckmong/ubuntuwithjava:ubuntuwithjava8
    expose:
      - "25565"
    ports:
      - "25565:25565"
    volumes:
      - ./modpackserverfolder:/home/modpackserverfolder
    restart: unless-stopped
    stdin_open: true
    tty: true

```

## Explainations 

Most of this section is for people newer to docker and need a hand with changing parts of the compose file to suit their needs.

Attach the path to your modpack server folder to a location in the container so that files will persist.

If you wish to change the port that the server uses in order to host multiple minecraft servers for example, change the `"25565:25565"` under `ports` to `"desiredport:25565"`. Now the server port will be located at the desired port number.

The restart: unless-stopped section sets the container to relaunch in the case that it is closed by an error or otherwise not by the user. This can be removed if you don't want this behaviour.

If you wish to use this for a game or application other than minecraft, you will need to change and add ports to the expose and ports sections.

If you're having issues and your not awfully experienced with docker-compose (like I was), please check out Docker's Compose file 3 reference documentation found here https://docs.docker.com/compose/compose-file/compose-file-v3/. This documentation has helped me a lot when I was starting out.

## Running a command on container startup without exec'ing into the container

In the case you want your server to start when the container starts, add the following to the compose file.
```
  command: bash /home/PathToModpackFolder/launch.sh
```

# Versions

The names of these versions are the base image and the java version. They also double as the tag names.

Versions:
- ubuntuwithjava8
- ubuntuwithjava21
- alpinewithjava8
- alpinewithjava21

If you're using this container for minecraft servers of anykind, Minecraft versions 1.12.2 and below need java 8 and any other versions are fine with java 17 and above.

# Running a minecraft modpack server

This section is a quickstart guide to using this container for a minecraft modpack server. This also works as a very liberal example for running other apps with this container

Do `docker exec -it <container name> bash` to enter the container. For alpine images you'll need to replace `bash` with `sh` anytime it's mentioned.

Navigate to the location of your modpack server files. If you followed the provided examples, it'll be in /home/modpackfiles with modpackfiles being the name you gave it.

Do `bash launch.sh` where launch.sh is the name of the shell script to launch the server.

## Server launch script

A Minecraft modpack server launch bash script can be found in the below link in the case the modpack server files don't include one.
https://github.com/Nomifactory/Nomifactory/blob/f05d2f552ca8441c3f26fff76e16392c74f12337/launchscripts/launch.sh
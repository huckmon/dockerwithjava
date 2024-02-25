# Introduction

This is a container I made to host minecraft modpack servers quickly and switch between different modpacks. Also a good excuse for me to learn how to build containers.

I am not an expert at docker usage so please take most of this documentation with a grain of salt.

Quick notes
- The provided container images don't expose ports by default, this guide somewhat covers this
- There is no latest tag, you'll need to select the tag that best suits your use case
- All container images are built with nano installed should you need to edit files

# Docker quick start

``docker run -d -it -expose "25565" -p 25565:25565 -v ./modpackserverfolder:/home/modpackserverfolder huckmon/ubuntuwithjava:ubuntuwithjava8``

Very basic docker command to run the container. Refer to the docker-compose section for more indepth stuff.

# Docker-compose quick start

Example compose file 

```
version: "3.8"

services:
  ubuntuwithjava8:
    image: huckmong/ubuntuwithjava:ubuntuwithjava8
    expose:
      - "25565"
    ports:
      - "25565:25565"
    volumes:
      - ./modpackserverfolder:/home/modpackserverfolder # your modpack server folder
    restart: unless-stopped
    stdin_open: true
    tty: true

```

If you wish to use this for a game or application other than minecraft, you will need to change and add ports to the expose and ports sections.

In the case that you are running this for a minecraft modpack server, I recommend not running detached for the initial server launch as the initial launch takes much longer than subsequent launches.

If you're having issues and your not awfully experience with docker-compose (like I was), please check out Docker's Compose file 3 reference documentation found [here](https://docs.docker.com/compose/compose-file/compose-file-v3/). This has 

## Running a command on container startup without exec'ing into the container

In the case you want your server to start when the container starts, add the following to the compose file.
```
  command: bash /home/PathToModpackFolder/launch.sh
```

# Running a minecraft modpack server

This section is a quickstart guide to using this container for a minecraft modpack server. This also works as a very liberal example for running other apps with this container

Do ``docker exec -it <image name> /bin/bash`` to enter the container.

Navigate to the location of your modpack server files. If you followed the provided examples, it'll be in /home/modpackfiles with modpackfiles being the name you gave it.

Do ``bash launch.sh`` where launch.sh is the name of the shell script to launch the server. Look at the section below if you're missing one.

If you want 

# Versions

The names of these versions are the base image and the java version. They also double as the tag names.

Versions:
- ubuntuwithjava8
- ubuntuwithjava21
- alpinewithjava8
- alpinewithjava21

If you're using this container for minecraft servers of anykind, Minecraft versions 1.12.2 and below need java 8 and any other versions are fine with java 17 and above.

# Server launch script

A Minecraft modpack server launch bash script can be found here in the case the modpack server files don't have one
https://github.com/Nomifactory/Nomifactory/blob/f05d2f552ca8441c3f26fff76e16392c74f12337/launchscripts/launch.sh
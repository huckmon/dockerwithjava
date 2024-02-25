# Introduction

This is a container I made to host minecraft modpack servers quickly and switch between them whenever I want.

I am not an expert at docker usage so please take most of this documentation with a grain of salt.

# Docker quick start

`` docker run -d -it -expose "25565" -p 25565:25565 -v ./modpackserverfolder:/home/modpackserverfolder huckmon/ubuntuwithjava:ubuntuwithjava8 ``

Very basic docker command to run the container.
# Docker-compose quick start

example compose file 

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

If you wish to use this for something other than minecraft, you need to change and/or add more ports to the expose and ports sections.

In the case that you are running this for a minecraft modpack server, I recommend not running detached for the initial server launch as the initial launch takes much longer than subsequent launches. 
# Versions

The names of these versions are the base image and the java version. they also double as the tag name.
Versions:
- ubuntuwithjava8
- ubuntuwithjava21
- alpinewithjava8
- alpinewithjava21

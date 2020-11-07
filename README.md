# docker-roms- Docker image to run the Regional Ocean Modeling System - ROMS

Creating a docker image, based on ubuntu 16.04, to run Regional Ocean Modeling System - ROMS, under any version of linux.


This docker image was based on the one found on [MetOcean's repository](https://github.com/metocean/docker-roms-public), with some simplification
and adapted by @nilodna for all Hydrodynamics Lab fellows.

## General comments

There is two types configuration to use docker4roms. The first is specific for personal computers, where the user has root privileges. The second one was designed to run
in the shelfwaves workstation, sharing a common ID for a specific docker group, allowing the researches to run their numerical simulations and still have access to the 
netCDF files generated.

## Pre-requisites

- Install docker in your system. See [here](https://docs.docker.com/engine/installation/).

## Getting started - development building setup

If you're a final user of this service, please skip to the **Getting started - final user** section.

Clone this repository and then type in your command line:

```
sudo docker build . -t docker4roms -f ${PWD}/Dockerfile.personal
```

To access creating a directory connecting host/guest, use:

```
docker run -v /path/to/a/shared/folder/projects:/home/lhico/projects -ti docker4roms
```

## Getting started - final user

Close this repository or just download the ```docker-compose-*.yml``` of your preference, but please be aware of the limitations for each setup.

### How to run?

After download or clone this repository, access the place where the ```docker-compose-*.yml``` file is and type in your terminal:

```
docker-compose -f docker-compose-shelfwaves.yml up -d
```
or 

```
docker-compose -f docker-compose-personal.yml up -d
```

With this command, the docker-compose software will build the right docker4roms release to your case, create two shared folder between host and container, sharing the ROMS source code and your projects setup and put all this to run in background (```-d```). 

To access the container, you can type:

```
docker exec -it roms bash
```

and you will access the working directory ```home/lhico``` with two folders: roms_src and projects.


## Running my first test case - upwelling




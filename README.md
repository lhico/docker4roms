# docker4roms- Docker image to run the Regional Ocean Modeling System - ROMS

Creating a docker image, based on ubuntu 20.04, to run Regional Ocean Modeling System - ROMS, under any version of linux.


This docker image was based on the one found on [MetOcean's repository](https://github.com/metocean/docker-roms-public), with some simplification
and adapted by @nilodna for all Hydrodynamics Lab fellows.

## General comments

There are two types of configurations to use docker4roms. The first is specific to personal computers, where the user has root privileges. 

## Pre-requisites

- Install docker in your system. See [here](https://docs.docker.com/engine/installation/).
- Right folder structures:
	- your_folder/docker4roms: this repository's folder
	- your_folder/projects: folder with your projects to run
	- your_folder/src_code: you must download the ROMS source code here

## Getting started - development building setup

If you're a final user of this service, please skip to the **Getting started - final user** section.

Clone this repository and then type in your command line:

```
sudo docker build . -t docker4roms -f ${PWD}/Dockerfile.personal
```

To access the container with a shared folder between host/guest, use:

```
docker run -v /path/to/a/shared/folder/projects:/home/lhico/projects -ti docker4roms
```

## Getting started - final user

Clone this repository or just download the ```docker-compose-personal.yml```, but please be aware of the limitations for each setup.

### How to run?

After download or clone this repository, access the place where the ```docker-compose-personal.yml``` file is and type in your terminal:

Notice that the following option allows multiple containers from a single image. You can replace `roms_user`, but you should also alter it in `docker-compose-personal.yml` in the `container_name` field.

```
docker-compose -f docker-compose-personal.yml -p roms_user up -d
```

With this command, the docker-compose software will build the right docker4roms release to your case, create two shared folders between host and container, share the ROMS source code and your project setup, and put all this to run in the background (```-d```). 

Note that the container building process will create two new folders on ```../``` of your structure. These two folders are fundamental to the Docker4ROMS usability, holding the source code of ROMS and your projects (numerical experiments). Make sure to copy the ```build_roms.sh``` file to the projects before accessing the container.

To access the container, you can type:

Again `roms_user` must be the same as in the docker-compose command.
```
docker exec -it roms_user bash
```

and you will access the working directory ```/home/lhico``` with two folders: roms_src and projects.

## Running my first test case - upwelling

This repository contains a ```build_roms.sh```pre-configure to be used inside the docker4roms structure. You must copy this file inside your ```projects``` folder and run the following steps for the UPWELLING test case:

```
# outside the container, first follow this sequence of codes:
cd ../projects 
mkdir upwelling 	# create new folder for our test
chmod 777 upwelling # give full writing and reading privileges
cd upwelling		

# copy files for the UPWELLING test case
cp ../../docker4roms/build_roms.sh .
cp ../../src_code/ROMS/External/roms_upwelling.in .
cp ../../src_code/ROMS/External/varinfo.yaml .
cp ../../src_code/ROMS/Include/upwelling.h .

# Now, access your container:
docker exec -it roms_user bash

# Once inside the container, go to the projects folder and create a new folder
# Note that the build_roms.sh must be in the projects folder.
cd projects; mkdir upwelling

# Now you need to copy the upwelling configuration files, that came with the ROMS source code
cd upwelling
cp ../build_roms.sh .
cp ../../roms_src/ROMS/External/roms_upwelling.in  .
cp ../../roms_src/ROMS/External/varinfo.dat .
cp ../../roms_src/ROMS/Include/upwelling.h .

# compile the model:
sudo bash build_roms.sh

# change VARNAME = varinfo.dat in the roms_upwelling.in
nano roms_upwelling.in

# now just run the model:
sudo ./romsS < roms_upwelling.in
```

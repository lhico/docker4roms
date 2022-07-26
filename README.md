# docker-roms- Docker image to run the Regional Ocean Modeling System - ROMS

Creating a docker image, based on ubuntu 20.04, to run Regional Ocean Modeling System - ROMS, under any version of linux.


This docker image was based on the one found on [MetOcean's repository](https://github.com/metocean/docker-roms-public), with some simplification
and adapted by @nilodna for all Hydrodynamics Lab fellows.

## General comments

There is two types configuration to use docker4roms. The first is specific for personal computers, where the user has root privileges. The second one was designed to run
in the shelfwaves workstation, sharing a common ID for a specific docker group, allowing the researches to run their numerical simulations and still have access to the 
netCDF files generated.

## Pre-requisites

- Install docker in your system. See [here](https://docs.docker.com/engine/installation/).
- Right folder structures:
	- your_folder/docker-roms: this repository's folder
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

and you will access the working directory ```/home/lhico``` with two folders: roms_src and projects.


## Running my first test case - upwelling

UNDER DEVELOPMENT

```
Rodando um test case
  Compilar o modelo
    Para o caso do upwelling (testcase mais conhecido), 3 arquivos são necessários. Estes já estão presentes no ROMS, mas fica cada um numa pasta
    dentro da estrutura.
    No diretório projects e cria uma pasta lá, pode ser upwelling mesmo
        cd projects; mkdir upwelling
        cp ../src_code/ROMS/External/roms_upwelling.in  .
        cp ../roms_src/ROMS/External/varinfo.dat .
        cp ../src_code/ROMS/Include/upwelling.h .
        cp ../src_code/ROMS/bin/build_roms.bash .
    faz esses passos aí dentro da pasta que vc criar pra simulação
    aí precisa editar alguns arquivos pra ajustar os caminhos.
    editar o build_roms.bash
      linha 110: mudar para MY_ROOT_DIR=${HOME}/roms_src
      linha 111: mudar para MY_PROJECT_DIR=${HOME}/projects/upwelling
      linha 124: mudar para MY_ROMS_SRC=${MY_ROOT_DIR}
      linha 173: mudar para FORT=gfortran
      linha 177: comentar a linha # export         USE_LARGE=on  (checar se isso é necessário)
      linha 178: descomentar a linha  export       USE_NETCDF4=on
    editar o roms_upwelling.in
      linha 76: mudar para VARNAME = varinfo.dat
  Rodando o modelo
    sudo ./romsS < roms_upwelling.in
```

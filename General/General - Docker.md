#general #docker

These notes are adapted from the 1hour crash course video by TechWorld with Nana found [here](https://www.youtube.com/watch?v=pg19Z8LL06w).

## The what and the why of Docker
Simply put it is a virtulaisation software that makes developing and deploying apps easier. Docker packages the application with all necessary dependencies, configs system tools and runtimes to ensure that it will run just like it did on the dev machine, anywhere you need it to.

Essentially, it turns your application into a simple, portable artifact that can be run anywhere.

### What problems does docker solve?
Before containerization became widespread, devs had to install and configure all services directly on the OS of their local machine. Think something like postgres, redis, a messaging service etc. It all adds up quickly to lots of config and the process can be different depending on OS choices. Theses services also then all need to be kept up to date across all the different dev devices. The chances of fuck ups then get really high.

With docker, you do not need to install all the dev services locally, they are instead packaged in individual, isolated environments. Instead of installing postgres, you can just fetch a postgress container from the internet and spin it up on your machine. The container will come packaged with everything it needs, and can be started on the local machine with the same command regardless of what OS you are running or anything else.

**DOCKER STANDARDISES THE PROCESS OF RUNNING ANY SERVICE FOR YOUR LOCAL DEV ENVIRONMENT**.

This also crucially makes it easy to run multiple different versions of the same service on the same machine without dealing with all the errors.

**DOCKER ALSO MAKES DEPLOYMENT WAY EASIER, BECAUSE NOTHING NEEDS TO BE INSTALLED AT THE SYSTEM LEVEL ON THE SERVER ASIDE FROM DOCKER ITSELF**.

## Docker vs virtual machines
Docker is a virtualisation tool, just like VM's are. It does however have some critical differences and advantages. To understand these we need to look a little at how docker works, starting with some OS basics.

### Basic OS makeup
An OS has two major layers, the kernel and the applications layer. The kernel is the part that communicates with the hardware components of the system and allocates resources to the software running in the applications layer. Essentially the kernel is the middle man between applications which run in the app layer, and the hardware that has computing resources to run them.

The main difference between docker and vms is which part of the OS they actually virtualise. Docker virtualises the applications layer of the OS, and some services that are installed on top of it like the python runtime for example. It then uses the kernel of the host machine, because it does not have it's own. Traditional VMs in contrast, will virtualise the entire OS system, including the kernel layer.

This leads to the key differences between the two technologies:
- Docker images are much smaller (MBs vs GBs)
- You can start docker images way faster due to the lack of a kernel layer (secs vs mins)
- VMs are compatible with all OS's, but docker is only compatible with linux systems, becuase it relies on the linux kernel and does not deploy it's own kernel (docker desktop now exists but who cares, not me.)



## Install Docker locally

## Images vs containers

## Public and private registries

## Running containers

## Create your own image (dockerfile)

## The major Docker commands

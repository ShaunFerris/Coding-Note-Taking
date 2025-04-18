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
- VMs are compatible with all OS's, but docker is only compatible with linux systems, becuase it relies on the linux kernel and does not deploy it's own kernel (docker desktop now exists and uses a hypervisor containing a linux distro to make it work on windows but who cares, not me.)

## Install Docker locally
To get started with docker, just go to their site and follow the instructions. They differ by OS and change somewhat frequently so typing them out here would be a boondoggle. You can install only what components you need, or you can install "docker desktop" which includes:
- docker engine - the main part that runs the containers
- docker CLI client
- docker GUI client
- Kubernetes and a bunch of other stuff
Remember that if you are on mac or windows then you NEED to install docker desktop, but on linux you can get by just installing the engine and CLI, as you already have the linux kernel and do not need the hypervisor layer.

## Images vs containers
Just like other applications can be packaged as .zip or .tar artifact files to make them easier to share and move around, docker does this with you application. The file is called a docker image. The docker image is an executable artifact file containing the compiled app code and all the other dependancies and runtimes you need. 

If that's an image, what is a docker container? It is what actually starts the application. The docker runtime takes an image and spins it up into a container. A container is thus just a running instance of a container. You can run multiple containers from a single docker image which is useful for loadbalancing.

Using the docker-CLI tool which is installed alongside the engine/docker desktop, you can list images with `docker images` and you can list running containers with `docker ps`.

## Public and private registries
So how do we go about getting images to run as containers on our system? This is where docker registries come from. A registry is online storage for preconfigured ready to rock docker images. Note that a registry is the service where repositories of docker images are hosted. These are where you go if you want the official image for mongoDB or Redis or Postgres etc. Docker maintains "dockerhub", the largest official image repository.

There are also private registries where you have to sign up and often pay, where you can get more bespoke images. Or your organisation might pay a cloud company for a service to store their own internal docker images.

Of course, as the app you want is updated the image is also updated, so if you need a specific version of postgres, you need to make sure you specifiy the version of the dockerimage. All images have the latest tag if you just want the most recent version. If you don't specify the version you will always get the latest version.

## Running containers
To actually get and run an image, you should search for it on dockerhub to get the correct name. Then on your local machine, run `docker pull <image-name>:<image-version>`. This will download the image from the registry (dockerhub is the default location, you have to tell it if you want to pull from a different repo). If you run for example `docker pull nginx:1.23` and then run `docker image` you should see the correct image listed. 

Now to run an image as a container, you need to execute `docker run <image-name>:<version-tag>`. Now the container is started and should show up when you run `docker ps`. This container will persist and keep running for as long as the system does, unless you tell it to stop. Note that the terminal session you run it from will be hung up, unless you run it in headless mode with the `-d` flag. Headless mode will mean that you cannot see the logs directly in terminal though. To see the logs after starting in headless mode, run `docker logs <container-id>`.

To stop a running container you can use `docker stop <container-id>`

So far what we have done is pull an image down so that it is available locally, and then run it from local. We can instead run an image directly with the run command and docker will automatically pull it down from docker hub. 

### Port bindings
Applications inside the docker container run in an isolated docker network. We need to expose the container port to the host machine to be able to actually interact with it. We do this with a port binding.

Applications that listen for connections have a standard port that the listen on. For example nginx runs on port 80 and redis runs on port 6379. This standard port is where the container is running. We need to map the localhost port to the container port the app is running on and we do this by adding the `-p` flag to our run command and then providing docker with a port-mapping of the format `<local-host-port>:<internal-docker-port>`.
For example to map localhost port 9000 to nginx, you would do something like this:
`docker run -d -p 9000:80 nginx:1.23`.

### Starting and stopping containers
The docker run command creates a new container from the image everytime. It does not re-use the previously created container. If you run `docker ps` you will only see the running containers, not containers that you created and then later stopped, however **THOSE CONTAINERS DO STILL EXIST**. If you run `docker ps -a` you will see the stopped containers as well as the running ones. The `docker start <container-id>` command will start existing containers that have been stopped. You can also use the container name in place of the id string for all of these commands. You can specify a new name for a container in the run comman using the `--name` flag. 

## Create your own image (dockerfile)
When you are done developing your application, which may rely on services that are dockerised, you will want to containerise your app too so that all aspects of the app are in their own neat little containers. 

To do this we need to package up our app into an image, and we do this by defining how we want this done in a dockerfile.  
![[Pasted image 20250409142437.png]]
Creating a simple dockerfile is really easy, and will allow us to create a docker image which can then be run as a container. 

Here we will step through creating a dockerfile to make an image from the simplest possible express app.

In the root of the application, create a new file called `Dockerfile`. Dockerfiles start from a a parent image or "**base image**". This is the existing docker image that your image will be based on. In the case of a simple express app, you would want the node base image, as express runs on top of node.js. The node base image is just a linux runtime with node installed.

We invoke the building of our image from the node base image using the `FROM` directive:
```Dockerfile
FROM node:19-alpine
```
Now our docker image will have access to node. It will not, however have express which is a dependancy, so it will not work. Because all docker images are based on a linux distro (alpine in this case) we can execute linux commands from the docker file, so we can have the dockerfile install express for us using the `RUN` directive. A good way to do this is to have it install all our dependacies from the `package.json` in the project root, and we can use the `COPY` directive for this:
```Dockerfile
FROM node:19-alpine

COPY package.json /app/

RUN npm install 
```
Now the dockerfile will copy the `package.json` from the project root into a `/app/` directory, and run `npm install` to install all dependancies listed there.

Obviously we need the source code for the project to also be in the image, so we should add another `COPY` directive:
```Dockerfile
FROM node:19-alpine

COPY package.json /app/
COPY src /app/

RUN npm install 

```
Note the the trailing slash in `/app/` is necessary so that docker knows to create the directory if it does not exist. 

The npm install command won't work from outside the directory containing the `package.json` so we need to use the `WORKDIR` directive:
```Dockerfile
FROM node:19-alpine

COPY package.json /app/
COPY src /app/

WORKDIR /app

RUN npm install 

```
This makes all following commands occur in the `/app` directory. Now we are ready for the final command, which is to actually start the app. We could do this with the `RUN` directive, but because it is the final directive in our dockerfile, we instead need to use the `CMD` directive to actually start the docker process. This directive takes an array where arr[0] is the command to run and arr[1] is the parameter:
```Dockerfile
FROM node:19-alpine

COPY package.json /app/
COPY src /app/

WORKDIR /app

RUN npm install 

CMD ["node", "server.js"]

```

## Building an image from the dockerfile
To build the image that we will run as a container, run `docker build -t <app-name>:<app-version-tag> <location-of-dockerfile>`. For example: `docker build -t my-app:1.0 .`

Often you will want to run multiple services from docker images to make up your app. To do this you can use docker compose, which was originally a plugin for docker but is now built into the engine. See this note for an LLM produced summary of docker compose: [[General - Docker Compse]]
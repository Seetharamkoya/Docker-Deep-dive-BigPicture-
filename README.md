# Docker-Deep-dive-BigPicture
Big picture on Docker by Nigel (summary)

## Big picture of Docker technology

Docker is an light weight microservices tool used to create, manage and Orchestrate containers.

Docker can run in linux and windows and there are three things to be aware of Docker technology.

1. The runtime
2. The daemon (Engine)
3. The orchestrator

## Docker Architecture

![image](https://user-images.githubusercontent.com/38424194/150133269-639f80ba-e66c-46b7-bf81-346e57eafcb8.png)

The low-level runtime is called runc and it's job is to interaface with the underlaying OS and start and stop containers.

The highlevel runtime is called containerd does a lot more than runc in the lifecycle of a container from pulling images, creating network interafaces and managing runc instances.

Docker Engine is a high level component sits on the Contained and perform high level tasks such as exposing the Docker remote API, managing volumes and Networking...

Docker also support in the managing the clusters of nodes runnig in the Docker. These clusters are called swarms/Docker-swarm.

## Installating Docker on ubuntu

> sudo apt-get update

> sudo apt-get install docker.io

Now, you can check the version

> sudo docker --version

## Docker Engine

![image](https://user-images.githubusercontent.com/38424194/150494049-92c2144f-cbb3-4433-a844-bd41daee2eed.png)

Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does heavy lifting of the building, running, and distrubting the docker containers. Both  can run on the same system or Docker client connect to a remote Docker daemon.
Docker client communities using the REST API, Over the unix scokets or a network interface. Docker compose is another docker client thats let you work with applications consisting of set of containers.

![image](https://user-images.githubusercontent.com/38424194/150528287-b909da1d-e594-4366-aa71-ea83dcff7bd8.png)


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

- The low-level runtime is called runc and it's job is to interaface with the underlaying OS and start and stop containers.

- The highlevel runtime is called containerd does a lot more than runc in the lifecycle of a container from pulling images, creating network interafaces and managing runc instances.

- Docker Engine is a high level component sits on the Contained and perform high level tasks such as exposing the Docker remote API, managing volumes and Networking...

- Docker also support in the managing the clusters of nodes runnig in the Docker. These clusters are called swarms/Docker-swarm.

## Installating Docker on ubuntu

> sudo apt-get update

> sudo apt-get install docker.io

Now, you can check the version

> sudo docker --version

## Docker Engine

![image](https://user-images.githubusercontent.com/38424194/150528287-b909da1d-e594-4366-aa71-ea83dcff7bd8.png)

> docker container run --name container image (names)
- When we type command into Docker CLI, the docker client converts them into the appropriate API playload and post them to the API end point exposed by the Docker dameon.
- The Docker Daemon receive instruction from the API endpoint  can be exposed to a local SOCKET or LOCAL NETWORK. on linux \var\run\docker.sock and on windows \pipe\docker-engine.
- Intially, daemon has no code to create container. Once it receives command it communicates with containered via CRUD-style API over gRPC.
- then **containered** will conert the image onto **OCI bundle** and instruct the **runc** to create a docker container using that image.
- **runc** interface with os kernel to pull necessary things to create a container and the container process started as child process of runc, and will exit once started.





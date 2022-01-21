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

> docker container run --name container image (names)
1. When we type command into Docker CLI, the docker client converts them into the appropriate API playload and post them to the API end point exposed by the Docker dameon.
2. The Docker Daemon receive instruction from the API endpoint  can be exposed to a local SOCKET or LOCAL NETWORK. on linux \var\run\docker.sock and on windows \pipe\docker-engine.
3. Intially, daemon has no code to create container. Once it receives command it communicates with containered via CRUD-style API over gRPC.
4. then **containered** will conert the image onto **OCI bundle** and instruct the **runc** to create a docker container using that image.
5. **runc** interface with os kernel to pull necessary things to create a container and the container process started as child process of runc, and will exit once started.

## How to implement on linux
• dockerd (the Docker daemon)
• docker-containerd (containerd)
• docker-containerd-shim (shim)
• docker-runc (runc)/

We can check the all this process using PS command on docker host.

## Securing the Client and daemon communication over network.

In the client-server model
> the client components implement the CLI
> 
> The server (daemon) component implement the functionality, including the public-fascing REST API.

By default, installation can be done on the same host and configure them to communicate over a local IPC socket on port 2375/tcp (HTTP).

![image](https://user-images.githubusercontent.com/38424194/150536627-01e823f5-9b8e-4036-944c-50c8fe2fb775.png)

But, for production environment this insecure configuration not be suitable instead use TLS secured. By allowing the communication with the trusted certificate authority(CA).

### SELF-SIGNED CERTS and CERTIFICATE AUTHORITY (CA)



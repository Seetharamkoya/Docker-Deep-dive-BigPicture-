# Docker-Deep-dive-BigPicture
Big picture on Docker by Nigel (summery)

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

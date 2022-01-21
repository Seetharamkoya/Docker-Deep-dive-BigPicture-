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

![image](https://user-images.githubusercontent.com/38424194/150494049-92c2144f-cbb3-4433-a844-bd41daee2eed.png)

Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does heavy lifting of the building, running, and distrubting the docker containers. Both  can run on the same system or Docker client connect to a remote Docker daemon.
Docker client communities using the REST API, Over the unix scokets or a network interface. Docker compose is another docker client thats let you work with applications consisting of set of containers.

![image](https://user-images.githubusercontent.com/38424194/150528287-b909da1d-e594-4366-aa71-ea83dcff7bd8.png)

> docker container run --name container image (names)
- When we type command into Docker CLI, the docker client converts them into the appropriate API playload and post them to the API end point exposed by the Docker dameon.
- The Docker Daemon receive instruction from the API endpoint  can be exposed to a local SOCKET or LOCAL NETWORK. on linux \var\run\docker.sock and on windows \pipe\docker-engine.
- Intially, daemon has no code to create container. Once it receives command it communicates with containered via CRUD-style API over gRPC.
- then **containered** will conert the image onto **OCI bundle** and instruct the **runc** to create a docker container using that image.
- **runc** interface with os kernel to pull necessary things to create a container and the container process started as child process of runc, and will exit once started.


## Securing the Client and daemon communication over network.

In the client-server model
> the client components implement the CLI
> 
> The server (daemon) component implement the functionality, including the public-fascing REST API.

By default, installation can be done on the same host and configure them to communicate over a local IPC socket on port 2375/tcp (HTTP).

![image](https://user-images.githubusercontent.com/38424194/150536627-01e823f5-9b8e-4036-944c-50c8fe2fb775.png)

But, for production environment this insecure configuration not be suitable instead use TLS secured. By allowing the communication with the trusted certificate authority(CA).

### SELF-SIGNED CERTS and CERTIFICATE AUTHORITY (CA).

1. Create a new private key for the CA
```
openssl genrsa -aes256 -out ca-key.pem 4096

```
we will have a new file in our current working directory called as ca-key.pem. which is private.

2. Use the CA’s private key to generate a public key (certificate).
```
openssl req -new -x509 -days 730 -key ca-key.pem -sha256 -out ca.pem

```
Now we have two files in your current directory: ca-key.pem and ca.pem. ese are the CA’s key-pair and form
the identity of the CA. At this point, the CA is ready to use.

#### Create a key-pair for the daemon

1. Create the private key for the daemon.
``
openssl genrsa -out daemon-key.pem 4096
<Snip>

```
Create a certificate signing request (CSR) for the CA to create and sign a certificate for the daemon. Be
sure to use the correct DNS name for your daemon node. e example uses node3.
```
openssl req -subj "/CN=node3" \
-sha256 -new -key daemon-key.pem -out daemon.csr
```
we will have 4 keys in our working directory. This one we called daemon.csr.

3. Add required aributes to the certificate.

this step creates a file that tells the CA to add a couple of extended aributes to the daemon’s certificate
when it signs it. ese add the daemon’s DNS name and IP address, as well as configure the certificate to
be valid for server authentication.
Create a new file called extfile.cnf with the following values. e example uses the DNS name and IP
of the daemon node in the lab from Figure 5.6. e values in your environment might be different.

subjectAltName = DNS:node3,IP:10.0.0.12
extendedKeyUsage = serverAuth

4. Generate the certificate.
this step uses the CSR file, CA keys, and the extfile.cnf file to sign and configure the daemon’s
certificate. It will output the daemon’s public key (certificate) as a new file called daemon-cert.pem
$ openssl x509 -req -days 730 -sha256 \
-in daemon.csr -CA ca.pem -CAkey ca-key.pem \
-CAcreateserial -out daemon-cert.pem -extfile extfile.cnf

At this point, you have a working CA, as well as a key-pair for node3 that can be used to secure the Docker
daemon.
Delete the CSR and extfile.cnf before moving on.

# Docker Setup and Enablement

**Table Of Contents**

1. [Docker Installation & Setup](#docker-setup-and-installation)
2. [Creating Image ](#creating-image)
3. [Using Images](#using-images)
4. [Using Containers](#using-containers)
5. [Using Volumes](#using-volumes)
5. [Using Network](#using-network)
6. [Using Composer](#using-composer)
7. [Using Swarm](#using-swarm)
8. [Service](#docker-services)

### Docker Setup and installation
Docker is a containerization tool that allows your app image to run on the docker engine. This docker engine provides an abstraction layer on top of the host machine's OS for our application. We can imagine it as JVM for the application which can compile Dockerfile and create images and can containerize it similar to an object from a class.

* Once the docker desktop is running on your local check the version using the below command
    > docker --version 

* **Note:** Docker desktop error on windows use this ``net localgroup docker-users JDNET\<userid> /add`` as admin in cmd 
		<br> ![image](https://github.deere.com/storage/user/31215/files/a5589827-492a-47d5-9005-1aac8b3ce8e0)
            	<br> You might need g90_docker_desktop_users AD group membeship, if yes please raise a Service now incident
### Creating Image
The file which can be compiled by a **docker engine** is a docker file that holds various instructions to create an image for your app. Every image is based on some other image which is called the base image of your image. Using various images and some instructions via the docker file we can also create an image for our application. The only thing we need to know is that we should be aware of all the dependencies of our application and should mention them in a particular order so that the image can be run or containerized. The running instance of an image is a container.
* Create a dir {some-app} in your workspace and the app you want to dockerize.
* Create Dokerfile and .dockerignore files inside the root directory of your app
* Dockerfile is the file that you can have in order to pass the instructions to docker to create an image of your application.
* .dockerignore file is to mention the files or directories need to be ignored when docker builds the image
* once your docker file is ready you build an image out of it using
    > docker build -t \<image-name>:\<version> .
* Once the image is built you need to tag it 
    > docker tag \<local-image>:\<version> respositoryUser\<image-name>:\<version>
* Login it to the docker repository
    > docker login -u \<user-name> -p \<password/secret-key>
* Push the image to repo
    > docker push respositoryUser\<image-name>:\<version>
* Pull the image
    > docker pull respositoryUser\<image-name>:\<version>

* Sample Dockerfile 
```
FROM ubuntu

RUN mkdir /iota-app
RUN echo "Hello! I will create image of ubuntu."
RUN apt update
RUN apt --version
RUN echo "Hello! Ubuntu machine initialized"

WORKDIR /some-dir
COPY /some-dir/. .
EXPOSE 4001
CMD ["echo", "Hello! welcome to ubuntu"]
```
* Sample .dockerignore file
```
.idea
.gitignore
README.md
``` 
### Using Images
Images are like the classes of the objects or blueprint of your app and all it needs to spin up should be mentioned in the Dockerfile so that an image can be created from it.

* Some commands related to images 
    > docker search \<image-name>

    > docker pull \<image-name>:\<version>

    > docker images -a

    > docker image list 

    > docker rmi \<image>:\<version>

### Using containers
The container is a running instance of an image (of your application). Whatever an image requires can be fulfilled by the docker engine and with all the dependencies(network, files, volumes, ports ) and image an application can be instantiated by the docker engine. This running instance is referred to as a container.

* Some commands related to containers 
    > docker run --name \<container-name> -it -d \<image>

    > docker run --name \<container-name> -it \<image>

    > docker run --name \<container-name> -it -p \<host-port>:\<container-port> \<image>

    > docker run --name \<container-name> -it --network \<network-name>  -- volume \<volume-name>:\<path-inside-container> -p \<host-port>:\<container-port> \<image>

    > docker attach \<container-name>

    > docker stop \<container-name>

    > docker start \<container-name>

    > docker rm \<container-name>
   

### Using Volumes
Volumes are like the attached storage of your container you can create a volume and can attach the same volume to multiple containers so that they use the same storage. Also these volumes are attached so they are not dependent on the container's life. No matter whether the container is running or not but volume will remain intact independently.

* Some commands related to volume
    > docker volume create --name \<volume-name> --opt type=none --opt device= \<path-inside-host> --opt o=bind
    > docker volume list 
    > docker inspect \<volume-name>


### Using Network

To create and communicate over a network among the containers we can create a network and attach the network to the containers so that can communicate and connect with each other.

* Some commands for network
    > docker network create  --driver=bridge \<network-name>

    > docker network list

    > docker inspect \<network-name>

    > docker network connect \<network-name> \<container-name>


### Using Composer

To spin up multiple container applications we can use composer which allows us to create **\<Docker-compose>.yml** file to mention details of various container applications in a single file. Using the composer **.yml** file we can spin up multiple containers in a particular order.

* Some important compose commands
    > docker compose up  -d daemon mode 

    > docker compose ps 

    > docker compose config

    > docker  compose -f filename up -profile rediscahe -d

    > docker compose exec service_name

    > docker compose stop

    > docker compose down

* Create \<some>.env file to define variables for the services


- Sample docker composer file

```
name: container name
	Service:
		app:
			build:
				dockerfile: Dockerfile
		web:
			image: "ngnx:version"
			ports:
				- "8000:80"
			networks:
				-my_network
			depends_on:
				-db
				-rediscache
		rcache:
			image: "redis:version"
			profiles:
				-rediscache
		db:
			image: mysql
			environment:
				- key=${value}
			env_file:
				- mysqlile.env
	network:
		my_network:
			driver: bridge
```



### Using Swarm

To create and set up a container orchestration we need to use swarm and organize the machine as manager(s) or worker(s) nodes. To create and use docker swarm you need at least two machine on which docker is enabled.

Once the docker is enabled and machines are available you can create a master node that will be managing the load among the worker nodes.
1. Go to the manager machine and run the following command to initialize the swarm service and generate a key for the worker nodes so that they can join the master machine (using IP address in the command). 
    > docker swarm init --advertise-addr=\< IP address of the master machine>
2. The above command will return the token and address to join as a worker node to the manager.
3. If you want to join join another manager(Secondary manager) to this manager you can use following command
    > docker swarm join-token manager

    **Note: In prod environments, we generally have multiple manager nodes**
4. Now switch to your worker machine and run the command below to join the manager as a worker node
    > docker swarm join --token \<your token goes here generated on step 1> \<managerIP>:\<port>
5. From your manager machine you can run a command to return the list of the machines running in orchestration mode (manager and worker) one of them should be manager at least
    > docker node ls

### Docker services

Instead of running a single container for each application, we need multiple replicas or instances of the same container. In docker, we can achieve "High availability, Scaling, Fault Tolerance, self-healing" capabilities via docker services.

1. To start the service instead of the container you can use the following command
    > docker service create --name \<some-name> --replicas \<mention-count> -p \<host-port>:\<container-port> \<image-name>
2. To List services
    > docker service ps \<service-name>
3. To scale services up
    > docker service scale \<service-name>=\<count>
    **Note: YOu can drain a node means no drain on that machine.**
4. To drain the node or active run the command
    > docker node update --availability active/drain \<Ip of the node>


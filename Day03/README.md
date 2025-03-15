
![03](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/099fe856-0a3f-4b60-b093-c240d20834f1)


## Table of Contents

1. [Introduction to Docker Containers](#introduction-to-docker-containers)
2. [Understanding Data Persistence](#understanding-data-persistence)
3. [Volumes vs Bind Mounts](#volumes-vs-bind-mounts)
4. [Practical Examples](#practical-examples)
   - [Using Volumes](#using-volumes)
   - [Using Bind Mounts](#using-bind-mounts)
5. [Docker Client and Daemon](#docker-client-and-daemon)
6. [Conclusion](#conclusion)

## Introduction to Docker Containers

Containers are stateless by nature, meaning if a container is deleted, all data within it is lost. This is where data persistence comes into play, ensuring your data remains intact even if the container is removed. We achieve this through Volumes and Bind Mounts.

## Understanding Data Persistence

**Persistence** means ensuring that even if a container is deleted, the data is not lost. There are two primary ways to achieve data persistence:

1. **Volumes**: Managed by Docker, ideal for data that needs to persist beyond the container's lifecycle.
2. **Bind Mounts**: Directly mounts a directory or file from the host system, useful for advanced scenarios.

## Volumes vs Bind Mounts

- **Volumes**:
  - Managed by Docker.
  - Stored in a part of the host filesystem which is managed by Docker.
  - Preferred method for data persistence.

- **Bind Mounts**:
  - Maps a file or directory on the host to a file or directory in the container.
  - More complex but provides flexibility to interact with the host system.

## Practical Examples

# docker run --rm -d --name mongodb -p 27017:27017 mongo:latest
# docker ps
# docker exec -it mongodb mongosh
test> show dbs;
add some data into it
test> db.helo.find()

now come outoff the container and stop the container and re create it, after recreate it you will no see the data as its persisten.

### Using Volumes

1. **List existing volumes**:

   docker volume ls


2. **Create a new volume**:

   docker volume create mongodb


3. **Run a MongoDB container with a volume**:

   docker run --rm -d --name mongodb -v mongodb:/data/db -p 27017:27017 mongo:latest


4. **Check running containers**:

   docker ps


5. **Insert some data**:

   docker exec -it mongodb mongosh
   > show dbs;
   > Insert some data
   > show dbs;
   > db.helo.find();


6. **Stop and start the container**:

   docker stop mongodb
   docker ps -a
   docker start mongodb
 exec -it mongodb mongosh

now data will be persist.


### Using Bind Mounts

1. **Run a container without network access**:

   docker run --rm -d --name troubleshootingtools --network none troubleshootingtools:v10
   docker exec -it troubleshootingtools bash
   try to run docker commands like docker ps or docker images , it will give the error as whenever we run the docker command so it will
   go to docker daemon through docker socket.
   
so now will mount the docker socket with container to check how bind mounts work

3. **Run a container with Docker socket mounted**:

   # docker run --rm -d --name app1 -v /var/run/docker.sock:/var/run/docker.sock --network none kiran2361993/troubleshootingtools:v1
   # docker exec -it app1 bash
   
   now run command docker ps command it will work.


5. **Inspect the container**:

   docker inspect app1


## Docker Client and Daemon

- **Docker Client**:
  - Sends commands to the Docker daemon using the Docker socket.
  - Receives responses from the Docker daemon.

- **Docker Daemon**:
  - Background service running on the host OS.
  - Manages Docker objects like images, containers, networks, and volumes.
  - Communicates with the Docker client.

## Conclusion

By understanding and using volumes and bind mounts, you can ensure data persistence in your Docker containers. This guide provides a solid foundation to start working with Docker and manage data effectively. Happy Dockerizing!

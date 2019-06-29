# Docker Notes:
1. Getting started with docker
```
docker run hello-world
```
2. To start with a default command. The command should exist in the container. This default command cannot be changed later.
```
docker run busybox ls
```
3. Listing running containers
```
docker ps
```
4. List of all containers ever created
```
docker ps --all
```
5. ```run``` command creates and starts a container. You can do it seperately.
```
docker create hello-world 
docker start -a [id]
````
  Here ```-a ```  logs output to the terminal. By default, ```run``` logs output.  
  
6. To completely remove all stopped containers
```
docker system prune
```
7. To get logs from a container. Doesn't restart container.
```
docker logs [id]
```
8. 


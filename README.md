# Docker-Kubernates
1. Getting started with docker
```
docker run hello-world
```
2. To execute a command after creating cotainer. The command should exist in the container.
```
docker run busybox ls
```
3. Listing running containers
```
docker ps
```
4. List of containers ever created
```
docker ps --all
```
5. Run command creates and starts a container. You can do it seperately.
```
docker create hello-world 
docker start -a [insert id here]
````
Here ```-a ``` logs output to the terminal. By default, ```run``` logs output.


Note: this is for my personal use only. It probably won't make much sense to anyone else.  
# Docker: Manipulating Containers
1. Getting started with docker
```
docker run [image]
docker run hello-world
```
2. To start with a default command. The command should exist in the container. This default command cannot be changed later.
```
docker run [image] [command]
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
docker create [image] 
docker start -a [id] 
----  
docker create busybox
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
8. To stop a container. This command gives the processes inside the container some time (SIGTERM) to do some clean up eg. produce logs, etc.
```
docker stop [id]
```
9. To kill a container. Container is killed instantly without any time to do clean up.
```
docker kill [id]
```
10. Executing commands in running containers.
```
docker exec -it [id] [command]
```
11.  ```-it``` flag is actually combination of two seperate flags. ```-i``` connects terminal to STDIN, STDOUT, and STDERR of the container, and ```-t``` beautfies outputs.  
  
12. Getting a shell in a Container
```
docker exec -it [id] sh
```
13. Starting with a shell, eg:
```
docker run -it [image] sh
```
14. Docker ```run``` with port mapping. Ports don't have to be identical.
```
docker run -p [local port]:[container port] [id/image]
docker run -p 8080:8080 monjurhasan/simpleweb
```

# Docker: Building Custom Images
1. To build a dockerfile. File should be in that directory. Logs an ID.
```
docker build .
docker run [id]
```
2. [Docker file example](https://github.com/monjur-hasan/Docker-Kubernates/blob/master/redis-image/Dockerfile)
```
from node:alpine

WORKDIR '/app'

COPY package.jon .
RUN npm install

COPY . .

cmd["npm", "run", "start"]
```
3. Tagging an image convention: dockerID/projectName:version
```
docker build -t monjurhasan/redis:latest .
docker run monjurhasan/redis
```
If no version is specified, latest one is pulled. Just the version on the end is the tag.

# Docker Compose
1. To build images
```
docker-compose up
```
2. To rebuild all the images inside
```
docker-compose up --build
```
3. Automatically creates a network that joins the containers  
  
4. Launch in background
```
docker-compose up -d
```
5. Stop all containers
```
docker-compose down
```
6. Automatic container restarts
--> "no": never attempts to restart if it stops/crashes //in quote
--> always: attempts to restart for  *any* reason
--> on-failure: only restarts if container stops with an error code // not 0
--> unless-stopped: always restart unless we forcibly stop it

7. Container status with compose. Need to run inside a directory containing yml file.s
```
docker-compose ps
```

# Production-grade Workflow
1. running with a custom dockerfile name
```
docker build -f Dockerfile.dev .
```
2. Docker volumes are used not to rebuild container evertime we change source code. [create-react-app example below]. -v arguements without ":" are not lookedup for reference. 
   
We deleted node_modules from 'pwd' because it's going to copy a large file. Instead we're just installing inside the container. Since docker volume always try to find reference in 'pwd' we are forcing docker to not look up refernces for node_modules
```
docker run -p [local-port:container-port] -v/app/node_modules -v $(pwd):/app [image_id]
```
It can be simplified using docker-compose.  
  
3. Simplying above command using docker-compose file:
```
version: '3'
services:
  web:
    build: 
      context: .
      dockerfile: Dockerfile.dev  # use this file from context path to build.
    ports:
      - "3000:3000"
    volumes:
      - /app/node_modules
      - .:/app
```
4. To run tests just overwrite default startup commands
```
docker run -it [id] npm run test
```
5. Docker compose for runnign tests. Instead of creating a seperate service
we can also run tests inside a single service but it requires ID of the container.
Neither of these two approaches can run interactive tests.
```
version: '3'
services:
  web:
    build: 
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - /app/node_modules
      - .:/app

  tests:
    build: 
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - /app/node_modules
      - .:/app
    command: ["npm", "run", "test"]

```
6. Multistep build example:
```
# temporary container
FROM node:alpine as builder 
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html

```
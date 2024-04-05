## Architecture Diagram:


![App Screenshot](https://github.com/Anup-Narkhede/Docker-Project-Mongo-Node-AWS-ECR-/blob/master/architecture.png)

#### Pull Docker images from docker hub
       ```
       docker pull mongo
       ```
       
       ```
       docker pull mongo-express
       ```
       
#### List images
```
docker images
```

#### Run both containers to make mongo db container available for our application

##Docker Network:
![App Screenshot](https://github.com/Anup-Narkhede/Docker-Project-Mongo-Node-AWS-ECR-/blob/master/docker-network.png)
     
 ### 1. Create Docker Network
 ```
 docker network create mongo-network
 ```
 
 #### Run mongo container from mongo image
 ```
 docker run -d \  
-p 27017:27017 \
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--name mongodb \
--net mongo-network \
mongo
 ```
 
 Check container logs
 ```js
 docker logs <container-id>
 ```
 
 #### Run mongo-express UI container from mongo-express image
 
 ```
 docker run -d \
-p 8081:8081 \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
--net mongo-network \
-e ME_CONFIG_MONGODB_SERVER=mongodb \
–name mongo-express \
mongo-express 
 ```
#### list currently running containers
```
docker ps
```
#### list all containers (stopped/running)
```
docker ps -a
```

### 2. Docker Compose:

![App Screenshot](https://github.com/Anup-Narkhede/Docker-Project-Mongo-Node-AWS-ECR-/blob/master/yaml1.png)

![App Screenshot](https://github.com/Anup-Narkhede/Docker-Project-Mongo-Node-AWS-ECR-/blob/master/yaml2.png)

#### Running yaml file:
```
docker-compose -f mongo-docker-compose.yaml up -d
```

```docker-compose <file> <filename> up <detached-mode>```

  
#### Stopping & removing container:
       
```
docker-compose -f mongo-docker-compose.yaml down -d
```

Note: docker-compose down also removes automatically created docker network

DOCKER-COMPOSE YAML CODE

```js
version: '3'
services:
    mongodb:
    image: mongo
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
  mongo-express:
    image: mongo-express
    ports:
      - 8081:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb
```
         

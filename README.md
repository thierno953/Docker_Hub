# Docker Hub

## Commands

- Note: USERNAME and PASSWORD hard-coded for Demo Purposes!

### Create Network

```
docker network create app-network --driver=bridge
```

### Pull Mongo image 

```
docker pull mongo
```

### Run Mongo Container

```
docker container run -d \
    -p 27017:27017 \
    -e MONGO_INITDB_ROOT_USERNAME=root \
    -e MONGO_INITDB_ROOT_PASSWORD=password \
    --network app-network \
    --mount source=app-volume,target=/data/db \
    --name mongo \
    mongo
```

### Viewing MongoDB Container Logs

```
docker logs mongo
```

### Pull Mongo-Express image

```
docker pull mongo-express
```

### Run Mongo-Express Container

```
docker container run -d \
    -p 8081:8081 \
    -e ME_CONFIG_MONGODB_ADMINUSERNAME=root \
    -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
    -e ME_CONFIG_MONGODB_SERVER=mongo \
    --network app-network \
    --name mongo-express \
    mongo-express
```

### Checking MongoDB Express Container Logs

```
docker logs mongo-express
```

### Create Volume

```
docker volume create app-volume
```

### Build app_server Image

```
docker build -t app_server .
```

### Run app_server Container

```
docker container run -it -d \ 
    -p 3000:3000 \
    --network app-network \
    --name app_server \
    app_server
```

### Build web_server Image

```
docker build -t web_server .
```

### Run web_server Container

```
docker run -it -d \
    -p 80:80 \
    --network app-network \
    --name web_server \
    web_server
```

### Push Images to Docker hub

```
docker login

docker image build -t thiernos/web_server:1.0 .
docker image push thiernos/web_server:1.0

docker image build -t thiernos/app_server:1.0 .
docker image push thiernos/app_server:1.0
```

### Run Docker Compose 

```
docker-compose.yaml up -d
```

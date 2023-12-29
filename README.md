# Commands

**Create networks**
```
docker network create app-network --driver=bridge 
docker network create web-server --driver=bridge 
docker network ls
```

**Create Volume**
```
docker volume create app-volume
docker volume ls
```

**Pull Mongo image**
```
docker pull mongo
```

**Pull Mongo-Express image**
```
docker pull mongo-express
```

**Run Mongo Container**
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

**I check the mongo logs**
```
docker logs mongo
```

**Run Mongo-Express Container**
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

**I run my mongo container on port 8081**
```
docker logs mongo-express
```

**Build app-server Image**
```
docker build -t app-server .
```

**Run app-server Container**
```
docker container run -it -d \
    -p 3000:3000 \
    --network app-network \
    --name app-server \
    app-server
```

**Build web-server Image**
```
docker build -t web-server .
```

**Run web-server Container**
```
docker run -it -d \
    -p 80:80 \
    --network app-network \
    --name web-server \
    web-server
```

**Push Images to Docker hub**
```
docker image build -t thiernos/app-server:1.0 .
docker image push thiernos/app-server:1.0

docker image build -t thiernos/web-server:1.0 .
docker image push thiernos/web-server:1.0
```

**Run Docker Compose**
```
docker compose -f docker-compose.yaml up
```

**To install Scout plugin**
```
curl -fsSL https://raw.githubusercontent.com/docker/scout-cli/main/install.sh -o install-scout.sh
sh install-scout.sh
```

**To See Docker scout manual**
```
docker scout --help
```

**Command provides an overview of the vulnerabilities found in a given image and its base image**
```
docker scout quickview thiernos/app-server:1.0
```

**Command gives you a complete view of all the vulnerabilities in the image**
```
docker scout cves thiernos/app-server:1.0
```

**This command supports several flags that lets you specify more precisely which vulnerabilities you're interested in, for example, by severity or package type**
```
docker scout cves --format only-packages --only-vuln-packages --only-severity critical thiernos/app-server:1.0

docker scout recommendations thiernos/app-server:1.0
```
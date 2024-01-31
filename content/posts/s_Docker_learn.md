---
title: "Docker"
date: 2023-12-26T14:59:55+08:00
author: "celine"
tags: ["Docker"]
categories: ["StudyNote"]
---

### Docker Note

docker ps 

​    list running containers



docker stop <container_id>

​    stops the container



docker start <container_id>

​    starts stopped container



docker ps -a

​    lists running and stopped container



docker run <image_name>:<version> 

​    pulls image and starts container 

​    (docker pull + docker start in one command)





[CONTAINER Port vs HOST Port]

\+ Multiple containers can run on your host machine

\+ Your laptop has only certain ports available

\+ Conflict when same port on host machine


| Port-binding |           |           |                       |
| ------------ | --------- | --------- | --------------------- |
| Host         | Port 5000 | Port 3000 | Port 3001             |
| Container    | Port 5000 | Port 3000 | Port 3000             |
|              |           |           | http://localhost:3001 |

綁定主機與container的port number  `docker run -p6000:6379 redis`

docker run -p<host_port_number>:<container_port> redis

以 detached mode run docker `docker run -p6000:6379 -d redis`

跑先前pull&start過的 redis:4.0 `docker run -p6000:6379 redis:4.0` -> port is already allocated

改用 host port 6001

`docker run -p6001:6379 -d redis:4.0`

> docker pull
>
> docker start
>
> docker stop
>
> docker run (pull + start) with options:
> `-d`    to run in detached mode
> `-p`    to bind port of your host to the container (-p\<hostport>:\<container_port>)
>
> docker ps `-a`  to give you all the containers no matter they are running currently or not
>
> docker images    to give you all the images that you have locally
>
> docker ps -> (check locks, outputs) -> (for toubleshooting) docker logs <container_id>
>
> docker logs <container_name>

docker logs \<container_id_or_name>

How to assign name for container

`docker run -d -p6001:6379 --name <custom_name> <container_image>`

docker exec 

​    to get the terminal of a running container 

​    to check the configuration file

​    to navigate into a directory inside

​    to check the log file or the configuration file 

​    to print out the environmental

`docker exec -it  <container_id> <directory_path>` or

`docker exec -it <container_name> <directory_path>`

​       `-it`     stands for interactive terminal 
​        docker exec -it e851017211f1 /bin/bash
​        env    印出環境變數
​        exit    離開container terminal

`docker network ls` 可以看到 auto generated network

(Network ID, name, driver, scope)

`docker network create <name_of_docker_network>`



安裝資料庫

+ docker pull mongo

+ docker pull mongo-express

+ docker images 確認有沒有確實安裝以上兩個 images

+ docker run -p27017:27017 -d mongo ... 以下是可以開始 run 即加上去的環境變數

  + MONGO_INITDB_ROOT_USERNAME
  + MONGO_INITDB_ROOT_PASSWORD
  + MONGO_INITDB_DATABASE

+ 加上初始化的環境變數，command就會變這樣

  + `-e` for environmental variables，可以 overwrite 預設值

  ```terminal
  docker run -p27017:27017 -d -e MONGO_INITDB_ROOT_USERNAME=admin MONGO_INITDB_ROOT -e MONGO_INITDB_ROOT_PASSWORD=secret mongo
  ```

+ 再加上兩個configurations 

  1. container name -- 用來跟 mongo express 連結
  2. docker network

  ```terminal
  docker run -d \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=secret \
  --name mongodb \
  --net mongo-network \
  mongo
  ```

+ 啟用 Mongo Express

  
  
  可以加入的環境變數主要有：
  
  1. ME_CONFIG_BASICAUTH_USERNAME
  2. ME_CONFIG_BASICAUTH_PASSWORD
  3. ME_CONFIG_MONGODB_ADMIN
  4. ME_CONFIG_MONGODB_ADMINUSERNAME
  5. ME_CONFIG_MONGODB_ADMINPASSWORD
  6. ME_CONFIG_MONGODB_PORT
  7. ME_CONFIG_MONGODB_SERVER
  8. ME_CONFIG_OPTIONS_EDITORTHEME
  9. ME_CONFIG_REQUEST_SIZE
  10. ME_CONFIG_SITE_BASEURL
  11. ME_CONFIG_SITE_COOKIESECRET
  12. ME_CONFIG_SITE_SESSIONSECRET
  13. ME_CONFIG_SITE_SSL_ENABLED
  14. ME_CONFIG_SITE_SSL_CRT_PATH
  15. ME_CONFIG_SITE_SSL_KEY_PATH

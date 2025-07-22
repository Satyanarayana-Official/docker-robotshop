### Docker_Project

1, Mongodb

Initializing a fresh instance
When a container is started for the first time it will execute files with extensions .sh and .js that are found in /docker-entrypoint-initdb.d. Files will be executed in alphabetical order. .js files will be executed by mongosh (mongo on versions below 6) using the database specified by the MONGO_INITDB_DATABASE variable, if it is present, or test otherwise. You may also switch databases within the .js script.

2, Catalogue

docker have two kind of networks bridge and host. host means directly host network. bridge means docker creates seperate network interface and assign the IP address to containers.
 
docker default bridge network can't communicate between containers, docker always suggest to create custom bridge network


docker network create <name of network>

[ ec2-user@ip-172-31-23-164 ~/docker-robotshop/catalogue ]$ docker network ls
NETWORK ID     NAME       DRIVER    SCOPE
79aa35b5a216   bridge     bridge    local
e14952433095   host       host      local
610e90d9d873   none       null      local
64de2747c412   roboshop   bridge    local

54.165.228.134 | 172.31.23.164 | t3.medium | https://github.com/Satyanarayana-Official/docker-robotshop.git
[ ec2-user@ip-172-31-23-164 ~/docker-robotshop/catalogue ]$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
catalogue    v1        62a5673677d2   19 minutes ago   1.25GB
mongodb      v1        a83beee9201f   2 hours ago      834MB

54.165.228.134 | 172.31.23.164 | t3.medium | https://github.com/Satyanarayana-Official/docker-robotshop.git
[ ec2-user@ip-172-31-23-164 ~/docker-robotshop/catalogue ]$ docker network disconnect bridge mongodb

54.165.228.134 | 172.31.23.164 | t3.medium | https://github.com/Satyanarayana-Official/docker-robotshop.git
[ ec2-user@ip-172-31-23-164 ~/docker-robotshop/catalogue ]$ docker network disconnect bridge catalogue

54.165.228.134 | 172.31.23.164 | t3.medium | https://github.com/Satyanarayana-Official/docker-robotshop.git
[ ec2-user@ip-172-31-23-164 ~/docker-robotshop/catalogue ]$ docker network connect roboshop mongodb

54.165.228.134 | 172.31.23.164 | t3.medium | https://github.com/Satyanarayana-Official/docker-robotshop.git
[ ec2-user@ip-172-31-23-164 ~/docker-robotshop/catalogue ]$ docker network connect roboshop catalogue

54.165.228.134 | 172.31.23.164 | t3.medium | https://github.com/Satyanarayana-Official/docker-robotshop.git
[ ec2-user@ip-172-31-23-164 ~/docker-robotshop/catalogue ]$ docker exec -it catalogue bash
root@986d5b8146da:/opt/server# curl http://localhost:8080/health
{"app":"OK","mongo":true}root@986d5b8146da:/opt/server#

3, Redis
    ``docker run -d --name redis --network roboshop redis:7``
    we can directly run this as we dont have any code for this

4, user
    same like catalogue as it is nodejs so i have configured only env variables

5, cart
    same like catalogue as it is nodejs project so i have ocnfigured only env variables

6, Mysql
    in this for shipping we need to create one roboShop@1 user
    also we need to run db scripts in mysql default path


7, shipping
    we need to copy files carefull otherwise it will not build the jar file
COPY pom.xml .
COPY src /opt/server/src
RUN mvn clean package

i have copied like this then only it worked so we need to makesure properly

if we want to build and run all the images at a time then we need to write one inline script like below

```for i in cart, catalogue,mongodb,mysql,shipping,user...etc; do cd $i; docker build -t vvsatyanarayana/$i:v1 . ; docker push vvsatyanarayana/$i:v1 ; cd .. ;done```

8, RABBITMQ
for this as per documentation we need to create one user and password 



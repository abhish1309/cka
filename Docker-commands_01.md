## *Docker engine/Docker Service*

### to check to status of docker engine/docker daemon
```
service docker status			
```

### after installation of docker, start the docker
```
sudo service docker start			
```

### to start docker at boot
```
sudo service docker enable			
```

## *Container* :-

### to check if docker is installed or not, and its information if installed
```
docker info			
```

### to check containers running
```
docker ps			
```

### to remove container
```
docker rm <container id/container name>			
```

### to get only container ids in output
```
docker ps -a -q			
```

### to delete all containers
```
docker rm $(docker ps -a -q)			
```

### to delete all containers
```
docker rm `sudo docker ps -a -q`			
```

## *Container Images* 

### to check docker images available
```
docker images			
```

### to get only docker images id
```
docker images -q			
```

### to delete none images.
```
docker images | grep none | awk '{print $3}' | xargs docker rmi			
```

### to search application images available in docker hub
```
docker search < application name >	
```
		
### to pull application images without running container
```
docker pull <application name>			
```

### to remove images
```
docker rmi <image id/ image name>			
```

### to delete all images
```
docker rmi $(docker images -q)			
```

### to delete all images
```
docker rmi `docker images -q`
```

## *Build container Images* :-			

### to create new image from current running docker
```
docker commit <running dockercontainerID name> < new image name>			
```

### to save the containers changes for future use.
```
docker commit 4aab3ce3cb76 jamtur01/apache2:webserver			
```

### to build from current location "Dockerfile" and tag
```
docker build . -t myuser/image2			
```

### "Dockerfile2" is the name of Dockerfile, build and tag to upload to Docker hub
```
docker build . -f Dockerfile2 -t myuser/image2	
```

### Push the image to registry
```
docker push registry/username/image.v1.1
```

## *Other Docker Commands* :- 

### to check the port of container
```
docker port <cont id/name>			
```

### to check the ports open or closed by using nmap (49152-65000 private ports)
```
nmap -p49000-50000 172.31.86.244			
```

### if there are multiple containers running, and you need the check the status of only required containers
```
docker inspect --format='{{ .State.Running }}' daemon_dave			
```
```
docker inspect --format '{{.Name}} {{.State.Running}}' \ daemon_dave bob_the_container
```

### to check the usage of resources of host machine by containers.
```
docker stats <c1-id/name> <c2-name> <c3-id>		
```

### to check open ports on machine
```
sudo netstat -tulpn			
```

### To install tomcat container on docker
```
docker run --name mytomcat1 -dp 8081:8080 tomcat:8.5.37-jre8			
```

### to install nginx
```
docker run --name mynginx1 -dp 8081:80 nginx			
```

### **Docker mode**
>* Attach mode		(without -d):- creates container with attached terminal. If terminal closed container will be stoped.

>* Detach mode		(with -d):- creates container with detached terminal

>* Interaction mode	(with -it):- enter into container to execute commads.

>* Detach + interaction	(with -dit):- runs in background

### to enter in container to execute required task
```
docker exec -it a5433244b81a /bin/bash
```

### to run the container in backgroud whitout exit, it will remain up until you stop it.
```
docker run -dit ubuntu	/bin/bash		
```

### to access the above container.
```
docker exec -it abc123 /bin/bash			
```

### to check differece between base image and current image
```
docker diff <container name>			
```

### to start a docker in restart mode, (when the ec2 is restarted, bydefault container comes in stopped state, to resart with daemon use below cmd)
```
docker run -d --restart unless-stopped --name myc2 -p 8083:80 nginx			
```

### to run mysql
```
docker run --name test-mysql -e MYSQL_ROOT_PASSWORD=DevOps@123 -d mysql:latest			
```

### to run wordpress link with mysql
```
docker run --name test3-wordpress --link test-mysql:mysql -p 8081:80 -d wordpress			
```

### to run wordpress normally
```
docker run --name test2-wordpress -p 8080:80 -d wordpress			
```

### to install docker compose
### docs docker compose link
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

### or DevOps tutor(Lx) link link
```
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

### to assign permission to use/execute docker compose
```
sudo chmod +x /usr/local/bin/docker-compose
```

### to check docker compose version
```
docker-compose version		
```

### to create and start container
```
docker-compose up -d			
```

### to check docker info
```
docker info			
```

### to check the port no accessible of container
```
docker port <container id>			
```

### to get the ip address of the system
```
ip r l
```
```
ip -br a
```


### to check the total size used 
```
docker system df
```
```
docker system df -v
```










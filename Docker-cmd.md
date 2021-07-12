# *Containers-Docker*

### To check to status of docker engine/docker daemon
```bash
service docker status 
```

### after installation of docker, start the docker
```bash
sudo service docker start			
```

### to start docker at boot
```bash
sudo service docker enable			
```

## *Containers* :- 
### to check if docker is installed or not, and its information if installed
```
docker info	
```
### to run container image
```
docker run nginx
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

## *Container Images* :-

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

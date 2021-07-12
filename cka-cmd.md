# Containers-Docker

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

## Containers :- 
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

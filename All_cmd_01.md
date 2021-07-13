# *Application Lifecyle management* :-


### to check the status of roll out
```
kubectl rollout status deployment/myapp-deployment		
```

### to check history
```
kubectl rollout history deployment/myapp-deployment		
```

### here the deployment.yaml file will not be changed, so different configuration will be there in current running pod and the file.
```
kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1		
```

### if update wont work, we can reverse the update.
```
kubectl rollout undo deployment/myapp-deployment		
```

### check command option.
```
kubectl describe pod ubuntu-sleeper		
```

# *Cluster Maintenance* :-


## *Join Node*

### print the link used to join the node to cluster.
```
kubeadm token create --print-join-command		
```
---
---
## *Os Upgrades*

### it simply marks the node as "unschedulable" and do not terminates the existing pods; it makes sure the new pods are not created on it.
```
kubectl cordon node-2		
```

### existing pods are terminated from the "node-1" and recreated on another node; and "node-1" is marked as cordan and unschedulable.
```
kubectl drain node-1		
```

### node-1 marked as uncordan to "schedule" pods.
```
kubectl uncordon node-1		
```

### We cannot delete/drain DaemonSet-managed pods; so we ingnored it.
```
kubectl drain node01 --ignore-daemonsets		
```


---
---
## *Kubernetes Software Versions* :-
### to check the version
```
kubeadm version
```

---
---

## *Cluster upgrade* :-
### to check the cluster info
```
kubectl cluster-info
```

### to check the current version
```
kubeadm version		
```

### to check the version in short
```
kubectl version --short		
```

### to check upgrade available
```
kubeadm upgrade plan		
```

---
---

## *Upgrade master node* :-
### to upgrade kubeadm & kublete to perticular versions 
```
kubectl drain controlplane --ignore-daemonsets
```
```
apt install kubeadm=1.18.0-00
```

```
kubeadm upgrade apply v1.18.0
```
```
apt install kubelet=1.18.0-00
```
```
kubectl uncordon controlplane
```

---
---

## *Upgrade worker node* :-

## from controlplane terminal.
```
kubectl drain node01		
```
```
ssh node01
```
```
apt install kubeadm=1.18.0-00
```
```
kubeadm upgrade node
```
```
apt install kubelet=1.18.0-00
```
```
exit
```
### from controlplane terminal
```
kubectl uncordon node01		
```

---
---

## *Backup and restore* :-
### to backup manually to the files.
```
kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml
```
 
### to get the backup of etcd.
### ### remember to specify --endpoints=" ",--cacert,--cert,--key  ; if running cmd on etcd which is on same master then no need apply endpoints.snapshotname: snapshot01.db; it is saved in pwd; specify path if required.
```
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
snapshot save /opt/snapshot.db 		
```

### snapshot.db ; files in /opt
```
ls /opt	
```

### check status
```
ETCDCTL_API=3 etcdctl snapshot status snapshot01.db		
```

###  stop kube-apiserver
```
service kube-apiserver stop		
```

### to restore ETCD cluster;	
```
ETCDCTL_API=3 etcdctl snapshot restore snapshot01.db --data-dir /var/lib/etcd-from-backup		
```

  * ### locate to manifest file location 
```
cd /etc/kubernetes/manifests/		 
```  
  * ### volumes >hostPath >path:/varlib/etcd-from-backup (its mandatory)
```
vi etcd.yaml 	
```
  * ### reload restart daemon
```
systemctl daemon-reload
```
  * ### restart etcd 
```
service etcd restart
```
  * ### start the kube-apiserver
```
service kube-apiserver start
```
# *ConfigMaps* :-

### create config maps 
```
kubectl create configmap app-configmap \
--from-literal=color=blue \
--from-literal=mode=prod
```

### pass values to file
```
echo -e "key1=value1\nkey2=value2" > config.properties		
```

### create config maps with the help of file.
```
kubectl create cm configmap2 --from-file=config.properties	
```

### imperetive method cm "webapp-color"
```
kubectl create configmap webapp-color --from-literal=APP_COLOR=darkblue		
```

### create config via file
```
kubectl create -f config-map.yaml		
```

### to get list config maps
```
kubectl get configmaps		
```

### to get list of configMaps
```
kubectl get cm		
```

### to get the yaml file of current running config "webapp-color"
```
kubectl get configmaps webapp-color -o yaml		
```

### cm "webapp-color"
```
Kubectl describe configmaps webapp-color		
```
```
kubectl edit configmap CONFIGMAP_NAME
```

### to get the format to update yaml file.
```
kubectl explain pods --recursive | grep envFrom -A3		
```
# *Cron Jobs* :-

### create cron jobs
```
kubectl create cronjob busybox --image=busybox \
--schedule="*/1 * * * *" \
-- bin/sh -c 'date; echo Hello from the kubernetes cluster'
```

# *DaemonSet* :-

### file same as replicaset only difference is kind, kind: DaemonSets
```
kubectl create -f daemonset.yml			
```

### to check the daemonsets present in current namespace (default)
```
kubectl get ds			
```

### to check daemonsets in all namespaces
```
kubectl get daemonsets --all-namespaces			
```
## *Deployments* :-


### Create a deployment without yaml file
```
kubectl create deployment mydeploy01 --image=nginx 		
```

### creates deployment and displays Deployment in YAML format (-o yaml). Don't create it(--dry-run)
```
kubectl create deployment mydeploy02 --image=nginx  --dry-run=client -o yaml	
```	

### Generate and save Deployment YAML file (-o yaml). Don't create it(--dry-run) , save to nginx-deployment.yaml
```
kubectl create deployment mydeploy03 --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml		
```

### create Deployment with 4 Replicas and records it
```
kubectl create deployment mydeploy04 --image=nginx --replicas=4 	
```

### create deployment with port 80, get it in file deployment.yaml
```
kubectl create deployment mydeploy05 --image=nginx --replicas=3 --port=80 --dry-run=client -o yaml > deployment.yaml
```

### create deployment set from existing file "nginx-deployment.yml"
```
kubectl create -f nginx-deployment.yml		
```

### record while creating
```
kubectl create -f mydeploy06.yaml --record		
```

***
***


### to check deployments
```
kubectl get deployments		
```

### to check replicaset created by deployment
```
kubectl get replicaset		
```

### to check pods created by deployment
```
kubectl get pods	
```

### to check all that has been created by deployments in one output 
```
kubectl get all		
```

***
***

###  to describe the deployment
```
kubectl describe deployments.apps mydeploy03 		
```

### record after edit
```
kubectl edit deployment mydeploy02 --record					
```

### scale deployment, replicaset updated without updating file
```
kubectl scale deployment mydeploy01 --replicas=3 		
```

### to expose port 
```
kubectl expose deployment mydeploy01 --port 80	
```

### record with imperative command
```
kubectl set image deployment mydeploy02 nginxcontainer=nginx:1.17 --record		
```

***
***

### to check rollout status
```
kubectl rollout status deployment mydeploy02		
```

#### to check history of rollout
```
kubectl rollout history deploy mydeploy02		
```

### to check history for specific revision
```
kubectl rollout history deploy mydeploy02 --revision=2		
```

### undo one step back
```
kubectl rollout undo deploy mydeploy02				
```

### undo to specific revision
```
kubectl rollout undo deploy mydeploy03 --to-revision=1		
```

### pause while rolling
```
kubectl rollout pause deploy mydeploy02			
```

### resume rolling
```
kubectl rollout resume deploy mydeploy02		
```

***
***

### autoscalling application /deployments
```
kubectl autoscale deployment mynginx --min=5 --max=10 --cpu-percent=80		
```

### get autoscalling deployments
```
kubectl get hpa mynginx
```

### delete autoscalling deployments
```
kubectl delete hpa mynginx
```
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

## *Build images*

### to create new image from current running docker
```
docker commit <running dockercontainerID name> < new image name>			
```

### to save the containers changes for future use.
```
docker commit 4aab3ce3cb76 jamtur01/apache2:webserver			
```

### to build from current location "Dockerfile" and tag the image
```
docker build . -t myuser/image2			
```

### to build from "Dockerfile2" (is the name of Dockerfile),tag to upload to Docker hub
```
docker build . -f Dockerfile2 -t myuser/image2	
```

### to push to image to local repo/ required repo
```
docker push x/y:z
```
# *Imperative & Declarative commands* :-

# Imperative 

### create objects
```
kubectl run --image=nginx nginx	
```

###
```
kubectl create deployment --image=nginx nginx
```

###
```
kubectl expose deploymet nginx --port 80
```

### update Ojects
```
kubectl edit deployment nginx		
```

###
```
kubectl scale deployment nginx --replicas=5
```

###
```
kubectl set image deployment nginx nginx=nginx:1.18
```

###
```
kubectl create -f nginx.yaml
```

### first edit the yaml file and then execute the command
```
kubectl replace -f nginx.yaml		
```

###
```
kubectl replace --force -f nginx.yaml
```

###
```
kubectl delete -f nginx.yaml
```


# Declarative:-

### create Objects
```
kubectl apply -f nginx.yaml		
```

### to create multiple objects at once. if multiple yaml file are present
```
kubectl apply -f /path/to/config-files		
```
### update Objects
```
kubectl apply -f nginx.yaml		
```
## *Jobs* :-

### create job
```
kubectl create job math-add-job --image=ubuntu -- expr 3 + 2
```

### get jobs details
```
kubectl get jobs
```

### check logs of jobs
```
kubectl logs job math-add-job-* 
```

### delete the job
```
kubectl delete job match-add-job-*
```
# *Labels & Selectors* :-

### show all labels of node node01
```
kubectl get node node01 --show-labels			
```
### show all labels of the existing pods
```
kubectl get pods --show-labels			
```

### to find pod with perticular label
```
kubectl get pods -l env=dev			
```
```
kubectl get pods -l env=dev,bu=finance
```

### to count no of pods in environment "dev"
```
kubectl get pods -l env=dev --no-headers | wc -l			
```

### to get the pods with the help of selector
```
kubectl get pods --selector app=App1			
```
# *Logging and Monitoring* :-

# Monitoring:-

### clone files of metrics-server from git
```
git clone https://github.com/kubernetes-incubator/metrics-server.git			
```

### creating a pod "metrics-server-" in kube-system for monitoring purpose; required pods/services gets created.
```
kubectl create –f deploy/1.8+/			
```

### monitor performance metrics of node
```
kubectl top node		
```

### monitor performance metrics of pods
```
kubectl top pod			
```

### to watch it live performance 
```
watch "kubectl top node"	
```

# Logging:-

### event stimulator for logs
```
kubectl create -f event-simulator.yaml			
```

```
kubectl logs -f event-simulator-pod
```

### specify name of teh container "event-simulator" if multiple containers are present in pod
```
kubectl logs -f event-simulator-pod event-simulator			
```

### refined/consized output for pod "webapp-1"
```
kubectl logs webapp-1 | grep -i user5			
```

### to check no of container present in the pod "webapp-2"
```
kubectl logs webapp-2 -c			
```

### check logs for container "simple-webapp" of pod "webapp-2" 
```
kubectl logs webapp-2 -c simple-webapp			
```
# *Multi-Scheduler* :- 

### to check the manifest files; kube-scheduler.yaml
```
cd /etc/kubernetes/manifests/
```

### sheduclar file
```
cat /etc/kubernetes/manifest/kube-scheduler.yaml	
```

### get events 
```
kubectl get events
```
# *Namespace* :-

### create a namespace "dev"
```
kubectl create namespace dev		
```
### to check the existing namespaces
```
kubectl get namespaces		
```
```
kubectl get ns
```

### check counts of namespace present
```
kubectl get ns --no-headers | wc -l		
```

### geting pods info from kube-system namespace
```
kubectl get pods --namespace=kube-system		
```

### short method to access from required namespaces
```
kubectl get pod	-n kube-system		
```

### get services from namespace "kube-system"
```
kubectl -n kube-system get svc		
```

## get deployments from namespace "kube-system"
```
kubectl -n kube-system get deployment 
```

### it will list all pods present in all namespaces.
```
kubectl get pods --all-namespaces	
```

### permenently switch to other namepace "dev". if we use get cmd for pods,deployment it will show details present in "dev" namespace only.
```
kubectl config set-context $(kubectl config current-context) --namespace=dev		
```
# *Networking* :-

Note:- some commands are valid till system restart. for perme

### list and modify interfaces of the host
```
ip link		
```

## to check ip addresses assigned to those interfaces
```
ip addr		
```

### to set the ip address to host machine
```
ip addr add 192.168.1.10/24 dev ehth0		
```

### to check the entries in routing table.
```
ip route		
```

### to check entries in routing table
```
route			
```

### to add ip routes
```
ip route add 192.168.2.0/24 via 192.168.1.1
```

```
ip route add default via 192.168.2.1
```

### 0. systems-A,B,C. A connected to B at eth0. B connect to C at eth1. system B with two network cards eth0 and eth1. etho0 does not send packets to eth1 if the value is 0.
```
cat /proc/sys/net/ipv4/ip_forward
```


### enable ip forwarding on host.
```
echo 1 > /proc/sys/net/ipv4/ip_forward		
```

### 1 ### command to check ip forwarding is enabled on the host
```
cat /proc/sys/net/ipv4/ip_forward		
```


# *DNS* :-


### add entry for DNS. this is also known as Name Resolution.
```
cat >> /etc/hosts		
```

note: if one of the hosts ip changes, then we need to configure it in all files in different systesm

### dns server ip maintained at this location of the host.
```
cat /etc/resolve.conf		
```

---
record types.
| Name  | type     |
|-------|-------   |
| A     | for ipv4 |
| AAAA  | for ipv6 |
| CNAME | for name change records.|


---

### it queries DNS server not local
```
nslookup		
```
### dig information
```
dig			

```

Network NS

### 
```
ip netns add red	
```
### cni plugins list
```
ls /opt/cni/bin  
```

### to check the networking solution used by cluster
```
cat /etc/cni/net.d/		
```

### to check teh logs of network plugins for ip address range.
```
kubectl -n kube-system logs weave-net-h2x9w -c weave	
```

```
kubectl logs kube-proxy-ft6n7 -n kube-system
```


### main file for coredns
```
cat /etc/coredns/Corefile		
```
# *Node Affinity* :-

### label the node first, and inject(add) the nodeaffinity in the pod spec of pod-definition.yaml file
```
kubectl label nodes node-1 color=blue		
```
# *Node Selector* :-

### syntax to label a node
```
kubectl label nodes <node-name> <label-key>=<label-value>			
```

### label node-1 "size=Large"
```
kubectl label nodes node-1 size=Large			
```

### get labels of node
```
kubectl get node node-1 --show-labels
```

### syntax to remove label of node
```
kubectl label node <nodename> <labelname>-
```

### to remove the label from node
```
kubectl label node node-1 size-
```

### test the run virtually without actualy creating it, tests if the pod gets creates or fails (dry run)
```
kubectl run mypod11 --image=nginx --dry-run=client		
```

### display the file generated by cmd used  // generate & display POD manifest Yaml file (-o yaml). Dont create it(--dry-run)
```
kubectl run mypod12 --image=nginx --dry-run=client -o yaml		
```

### generate a yaml file (pod.yaml) directly and then create pod using the file pod.yaml
```
kubectl run mypod13 --image=nginx --dry-run=client -o yaml > pod.yml		
```

### only pod definition file is created and container port is opened //create a pod "custom-mypod" using the nginx image and expose it on container port 8080
```
kubectl run mypod14 --image=nginx --port=8080		
```

### service & pod is created // Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80
```
kubectl run mypod15 --image=nginx:alpine --port=80 --expose		
```

### pod & svc definition is created in a single file // can create pod & svc from the file 
```
kubectl run mypod16 --image=nginx:alpine --port=80 --expose --dry-run=client -o yaml > pod-svc.yml		
```

### assign environment variable while creating pod
```
kubectl run mypod17 --image=nginx --env="environment=production" --env="department=finance"		
```

### assign labels while creating pod
```
kubectl run mypod18 --image=nginx --labels="app=myapp,tier=frontend"		
```

### assign labels while creating pod
```
kubectl run mypod18 --image=nginx -l app=myapp,tier=frontend		
```

### pass the argument by keeping the default command in container image.
```
kubectl run mypod19 --image=nginx <arg1> <arg2> <argN>		
```

### pass the command and argument, overriding the container image command.
```
kubectl run mypod20 --image=nginx --command -- <cmd> <arg1> <argN>		
```

### if true, service is created for the containers which are run
```
kubectl run mypod21 --image=nginx --expose=false		
```

### service account to set in the pod spec. this may depricate in future.
```
kubectl run mypod22 --image=nginx --serviceaccount=''
```

### annotations to apply to the pod.
```
kubectl run mypod23 --image=nginx --annotations=[]	
```

### one time use pod/container. pod do not restarts and gets deleted when we exit shell/ or when work is done.
```
kubectl run mypod24 --image=busybox -it --restart=Never --rm		
```

### it runs, execute the output, exits and pod is deleted.(ping and displays the output, gets deleted.
```
kubectl run mypod25 --image=busybox -it --restart=Never --rm -- wget -O- 10.1.1.131:80		
```

### passing command inside the container
```kubectl run mypod26 --image=busybox -it --restart=Never --rm -- sh -c 'wget -O- 10.1.1.131:80'	
```

### pod-definition.yml file which has information to create a pod.
```
kubectl create -f pod-definition.yml		
```

### check pods created
```
kubectl get pods	
```

### to get the ips of pod and other extra details
```
kubectl get pods -o wide	
```

### get pods from namespace kube-system
```
kubectl -n kube-system get pods		
```

### displays all pods including kube-system
```
kubectl get pods -A		
```

### get list of pods and services
```
Kubectl get pods,svc		
```

### get list of all objects. Deployment, replicaset, services, pods
```
kubectl get all		
```

### show pods with labels 
```
kubectl get pods --show-labels		
```

### get pods with the help of labels; purpose=demonstrate-envars 
```
kubectl get pods -l purpose=demonstrate-envars		
```

### to print the label values as a columns 
```
kubectl get pods --label-columns=app		
```

### get pods from all namespaces
```
kubectl get pods --all-namespaces		
```

### get pods using selector
```
kubectl get pods --selector='app=demo2'		
```

### Get all running pods in the namespace
```
kubectl get pods --field-selector=status.phase=Running		
```

### to get the yaml definition file of currently running pod.
```
kubectl get pod mypod -o yaml > nginx01.yaml		
```

### to get the ip of the pod
```
kubectl get pod mypod -o jsonpath='{.status.podIP}'		
```

### get pod name & images list of all pods
```
kubectl get pod -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.containers[*].image}{"\n"}{end}'	
```

### print in columns, with headers
```
kubectl get pod -o custom-columns='POD_NAME:.metadata.name,IMAGE:.spec.containers[*].image'	
```

### print in columns, without  headers
```
kubectl get pod -o custom-columns='POD_NAME:.metadata.name,IMAGE:.spec.containers[*].image' --no-headers		
```


### to inspect pod
```
kubectl describe pod mypod	
```

### to edit/update the configuration of the running pod 'redis'
```
kubectl edit pod mypod		
```


### to delete a pod "mypod"
```
kubectl delete pod mypod	
```

## delete the pod without wasting time
```
kubectl delete pod mypod --force --grace-period=0		
```

### delete pod using selector
```
kubectl delete pod --selector="app=demo"		
```

### delete the pod using labels
```
kubectl delete pod -l app=label		
```

### all the resources in the file present in cluster will be deleted.
```
kubectl delete -f pod.yaml		
```

### To delete pods in Failed state in namespace default
```
kubectl -n default delete pods --field-selector=status.phase=Failed		
```

### to label existing pod
```
kubectl label pod mypod app=demo	
```

### if label is already existing and you need to edit value, then use -overwrite
```
kubectl label pod mypod app=demo2 -overwrite	
```

### to remove the label, use key and - sign at the end of it.
```
kubectl label pod mypod app-		
```

### to add annotations for pod
```
kubectl annotate po mypod1 description='my description'		
```

### to add annotations for multiple pods.
```
kubectl annotate po mypod1 nginx2 nginx3 description='my description'	 
```

### to check logs of pods/containers
```
kubectl logs mypod		
```

### to tail the logs
```
kubectl logs -f mypod		
```

### if we want a logs of specific container inside the pod
```
kubectl logs -f podname -c containername	
```

### logs of previously running pod
```
kubectl logs mypod -p		
```

### to start a bash session in pods container ### sh, bash, /bin/sh, /bin/bash
```
kubectl exec -it $POD_NAME -- bash		
```

### to execute a command in a pod
```
kubectl exec mypod1 -- date		
```

### to execute command in container "nginx-container" of pod "mypod"
```
kubectl exec mypod -c nginx-container -- date	
```

### different attributes of pods.
```
kubectl explain pods			
```

### to make the changes,
```
kubectl explain pods.spec.containers		
```

### listen on port 8888(local port) locally, forwarding to 5000(container port) in the pod. 
```
kubectl port-forward pod mypod	8888:5000	
```

### to check,test application locally. () http://localhost:xxxx)

### kubetl set image POD/POD_NAME container_name=image:tag ( change the image on the fly.)
```
kubectl set image pod mypod nginxcontainer=nginx:1.7.1		
```
# cka
## *Replica Set* :-

### create replication set
```
kubectl create -f replicaset-definition.yml		
```

### check replicationset
```
kubectl get replicaset		
```

### edit virtually.
```
kubectl edit rs new-replica-set		
```

### first edit "replicas" property in *.yml file to scale replication set
```
kubectle replace -f replicaset-definition.yml		
```

### delete replicaset
```
kubectl delete replicaset myapp-replicaset		
```

### scale replicaset "new-replica-set" without updating data in file
```
kubectl scale replicaset new-replica-set --replicas=2		
```

### scale replicas by updating file & may need to apply to update
```
kubectl scale --replicas=6 -f replicaset-definintion.yml		
```
## *Replication Controller* :-

### create replication controller
```
kubectl create -f rc.yml	
```

### check no of replication controller
```
kubectl get replicationcontroller	
```
```
kubectl get rc
```
# *ResourceQuota* :-

### to create quota
```
kubectl create quota -h
```

### create pod with limited resources
```
kubectl run mypod --image=nginx --restart=Never \
--requests='cpu=250m,memory=64Mi' \
--limits='cpu=520m,memory=128'
```

### create quota
```
kubectl create quota myrq --hard=cpu=1,memory=1G,pods=2
```
# *Scheduling* :-

### to check if the kube-sheduler pod is present ### sheduler pod must be present with other pods
```
kubectl -n kube-system get pods			
```
# *Secrets* :-

## *Generic*

### imperative approach, to create secret directly from command; entering direct parameters
```
kubectl create secret generic <secret-name> --from-literal=<key>=<value>		
```
```
kubectl create secret generic app-secret \
--from-literal=DB_Host=mysql \
--from-literal=DB_User=root \
--from-literal=DB_Password=paswrd
```

### imperative approach using file
```
kubectl create secret generic <secret-name> --from-file=<pathToFile>			
```

### file "app_secret.properties"
```
kubectl create secret gereric app-secret --from-file=app_secret.properties		
```

### declarative approach, to create a secret from yaml file
```
kubectl create -f secret-data.yaml		
```

### to get list of secrets
```
kubectl get secrets		
```

### to describe newly created secret; without displaying value
```
kubectl describe secrets		
```

### shows the yaml file; with displaying values
```
kubectl describe secrets -o yaml		
```

### to edit the secret
```
kubectl edit secret SECRET_NAME		
```

### encoding value; to convert normal text "mysql" to encoded text "bX1zcWw="; to save encoded text in secret file instead of normal plain text.
```
echo -n 'mysql' | base64		
```

### decoding; to convert encoded text 'bx1zcWw=' to normal plain text 'mysql'
```
echo -n 'bx1zcWw=' | base64 --decode		
```

## *Docker-registry*

### create secret docker-registry 
```
kubectl create secret docker-registry regcred \
--docker-server=<your-registry-server> \
--docker-username=<your-name> \
--docker-password=<your-pword> \
--docker-email=<your-email>
```
# *Services* :-

### need to change the selector of service which matches deployment or pods/labels. to map service to deployment or pods.
```
kubectl create service clusterip mynginx-service --tcp=80:80 --dry-run=client -o yaml > nginx-service.yaml		
```

### need to change selector,create a nodeport service. If you dont define nodeport, then kubernetes will assign any available port.
```
kubectl create service nodeport mynginx-servie02 --tcp=80:80 --node-port=30080		
```

### need to change selector,create a loadbalancer
```
kubectl create service loadbalacer my-lbs --tcp=5678:8080				
```

### to create a service using yaml file
```
kubectl create -f svc.yaml	
```

### create a pod and service at the same time. it can only be used to expose pod.
```
kubectl run mypod02 --image=nginx --port=80 --expose		
```

### no need to change selector; pod must present to expose ##create as service redis-service to expose the redis application with the cluster on port 6379
```
kubectl expose pod mypod01 --name redis-service --type=ClusterIP --port=6379  	
```

### expose with cluster IP
```
kubectl expose deployment my-deployment02 --name=nginx-service --type=ClusterIP --port=80 --target-port=80		
```

### deployment must be present before exposing. and need to edit deployment to mention the nodeport, as we cannot set it directly through imp.command to generate a yaml file (svc.yaml) without creating a service
```
kubectl expose deployment my-deployment01 --name=webapp-service --type=NodePort --port=8080 --target-port=8080 --dry-run=client -o yaml > svc.yaml
```

### to expose deployment as loadbalancer service
```
kubectl expose deployment my-deployment02 --name=mylbs --type=LoadBalancer --port=80 --target-port=80		
```


### get the services details
```
kubectl get svc		
```

### describes the default svc "kubernetes"s
```
kubectl describe svc kubernetes		
```

### to check endpoints created by the service
```
kubectl get endpoints		
```

### to check endpoints connected by the service
```
kubectl get ep			
```

### to test the connection of service within pods. Can use ip address as well
```
kubectl run mybusybox01 --image=busybox -it --restart=Never --rm -- wget -O- http://<service-name>		
```
# *Static Pods* :-

### default static pods are in kube-system namespace
kubectl get pods -n kube-system

### to check static pods creation. because its indipendent from cluster.
```
docker ps		
```

### kubelete is responsible to run static pods
```
ps -aux | grep kubelet		
```

### copy the path of --config=/..../config.yaml
```
ps -aux | grep kubelet | grep .yaml		
```

### get the path of pod files location.
```
cat /..../config.yaml | grep -i staticPodPath		
```

---
### contains .yaml file // search --config on kubelet.
```
ps -aux | grep -i "\--config"		
```

### kubelet config file location; here the static pod definition file location is configured.
```
cat /var/lib/kubelet/config.yaml | grep -i staticpod		
```

### to list the files of default static pod definitions present in manifest folder.
```
ls /etc/kubernetes/manifests		
```
# *Taints and Tolerations* :-

### syntax; effect:-(NoSchedule | PreferNoSchedule | NoExecute)
```
kubectl taint nodes node-name key=value:taint-effect		
```

### untaint the node # - sign at end of key will untaint the node
```
kubectl taint node node-name key-		
```

### to taint on node with key-value
```
kubectl taint nodes node1 key1=value1:NoSchedule		
```

### to taint on node with key-value
```
kubectl taint nodes node1 app=blue:NoSchedule		
```

### need to try this command not sure, to taint controlplane with key only.
```
kubectl taint nodes controlplane node-role.kubernetes.io/master:NoSchedule		
```

## - sign at last removes taint on node
```
kubectl taint nodes controlplane node-role.kubernetes.io/master:NoSchedule-		
```
# *Troubleshooting* :-

1) check connectivity

>curl http://web-service-ip:node-port

>curl http://172.16.10.10:8181

2) check if pod is in running state
>kubectl get pod

3) check logs 
>kubectl logs web -f
>kubectl logs web -f --previous

4) check endpoints in namespace
>kubectl -n gama get ep

5) check environment variables.



# *Worker node failure* :-

## check if its ready
>kubectl get nodes			

## check if the status is true:-not healthy due to symtoms, false: healty
>kubectl describe node worker-1		

>top
>df -h 

>serive kubelet status
>sudo journalctl -u kubelet

>openssl x509 -in /var/lib/kubelet/worker-1.crt -text

1) check if the process for kubelet is running.
>ps -ef | grep -i kubelet		
>systemctl status kubelet.service
>systemctl restart kubelet

2) certificate issue
>sudo journalctl -u kubelet
>cd /etc/systemd/system/kubelet.service.d/
>cat 10-kubeadm.conf

>cat /../kubelet.yaml
	- check the certificate 
	- correct the certificate url

>systemctl daemon reload
>systemctl restart kubelet

3)
>cat 10-kubeadm.conf
	--kubeconfig=/etc/kubernetes/kubelet.conf


#6553 to 6443 port
>vi /etc/kubernetes/kubelet.conf## Persistent volumes

### pv - persistent volume available
```
kubectl get persistentvolume
```

### pvc - persistent volume claim available
```
kubectl get persistentvolumeclaim
```

# *Storage class* :-


## to get the storage class
```
kubectl get sc		
```

# *API Groups* :-

### get api resources details
```
kubectl api-resources
```

### to check the resources which are namespaced scoped
```
kubectl api-resources --namespaced=true   	
```

### to chekc resources which are cluster scoped
```
kubectl api-resources --namespaced=false  
```
# *CertificatesSignRequest* :-

### user generates the key
```
openssl genrsa -out jane.key 2048		
```

### user then create certificateSignRequest with the name on it and sends to admin
```
openssl req -new -key jane.key -subj "/CN=jane" -out jane.csr		
```

### convert value to base64 to use under request feild of yaml file( CSR object)
```
cat myuser.csr | base64 | tr -d "\n"		

```

### administrator takes a key and generates certificateSignRequest object (like other kubernetes object) with a manifest file.



#### Get list of CSRs
```
kubectl get csr		
```

### approve the CSR
```
kubectl certificate approve mysuser		
```

### Retrieve the certificate from the CSR; and manualy export from base64 ; select from certificate:   ;or else follow below cmd.
```
kubectl get csr/myuser -o yaml		
```


### The certificate value is in Base64-encoded format under status.certificate. Export the issued certificate from the CertificateSigningRequest.
```
kubectl get csr myuser -o jsonpath='{.status.certificate}'| base64 -d > myuser.crt		
```


# *kubeconfig* :-

### to check the no of clusters present.
```
kubectl config view   
```

### to check context
```
kubectl config view --kubeconfig=my-custom-config  
```

### to change context to production.
```
kubectl config use-context prod-user@production   
```

### list of supported commads
```
kubectl config  -h
```
# *Network policy* :-

### to get the network policy
kubectl get netpol   
# *RBAC* :-

### create role
```
kubectl --namespace=development create role developer --resource=pods --verb=create,list,get,update,delete
```

### roles available
```
kubectl get roles
```

### rolebindings available
```
kubectl get rolebindings
```

### describe role
```
kubectl describe role developer
```

### describe rolebinding
```
kubectl describe rolebinding devuser-developer-biniding
```

### authentication verify


```
kubectl auth can-i create deployments   
```
```
kubectl auth can-i delete nodes 
```

# to check access of user
```
kubectl auth can-i create deployments --as dev-user   
```

### to test access in namespace for user "dev-user"
```
kubectl auth can-i create pods --as dev-user --namespace test  
```


---
---

# *Cluster Role Bindings* :-


### create cluster role binding in perticular namespace
```
kubectl --namespace=development create rolebinding developer-role-binding --role=developer --user=john
```

### to check/get cluster roles present
```
kubectl get clusterroles		
```

### to check/get cluster rolebindings
```
kubectl get clusterrolebindings		
```
# *Security* :-

### user generate the key
```
openssl gerrsa -out my-bank.key 1024			
```

### user generate the CertificateSignRequest to send to admin
```
openssl rsa -in mybank.key -pubout > mybank.pem		
```

### to generate a certificateSignRequest
```
opensll req -new -key my-bank.key -out my-bank.scr -subj "/C=US/ST-CA/0=MyOrg, Inc./CN=my-bank.com"		
```

### Deployed Kubernetes from Scratch - the hard way
```
cat /etc/systemd/system/kube-apiserver.service			
```

### Deployed Kubernetes via kubeadm
```
cat /etc/kubernetes/manifests/kube-apiserver.yaml	
```


### to open and check the certificate details.
```
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -txt -noout		
```
### if from scratch; check logs with the cmd
```
journalctl -u etcd.service -l			
```

### if from kubeadm; logs followed by pod name (kube-system)
```
kubectl logs etcd-master			
```

### if not found via kubernetes; check one level down from docker, get the container id.
```
dcoker ps -a			
```

### check the logs of 87fc cont id.
```
docker logs 87fc		
```

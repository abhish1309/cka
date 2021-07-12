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
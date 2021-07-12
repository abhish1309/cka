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
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
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
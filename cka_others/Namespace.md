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
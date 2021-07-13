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

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

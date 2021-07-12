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

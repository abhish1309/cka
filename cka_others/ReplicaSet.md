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
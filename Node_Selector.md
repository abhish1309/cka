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
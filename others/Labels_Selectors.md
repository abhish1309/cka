# *Labels & Selectors* :-

### show all labels of node node01
```
kubectl get node node01 --show-labels			
```
### show all labels of the existing pods
```
kubectl get pods --show-labels			
```

### to find pod with perticular label
```
kubectl get pods -l env=dev			
```
```
kubectl get pods -l env=dev,bu=finance
```

### to count no of pods in environment "dev"
```
kubectl get pods -l env=dev --no-headers | wc -l			
```

### to get the pods with the help of selector
```
kubectl get pods --selector app=App1			
```
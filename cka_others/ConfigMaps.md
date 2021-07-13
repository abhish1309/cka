# *ConfigMaps* :-

### create config maps 
```
kubectl create configmap app-configmap \
--from-literal=color=blue \
--from-literal=mode=prod
```

### pass values to file
```
echo -e "key1=value1\nkey2=value2" > config.properties		
```

### create config maps with the help of file.
```
kubectl create cm configmap2 --from-file=config.properties	
```

### imperetive method cm "webapp-color"
```
kubectl create configmap webapp-color --from-literal=APP_COLOR=darkblue		
```

### create config via file
```
kubectl create -f config-map.yaml		
```

### to get list config maps
```
kubectl get configmaps		
```

### to get list of configMaps
```
kubectl get cm		
```

### to get the yaml file of current running config "webapp-color"
```
kubectl get configmaps webapp-color -o yaml		
```

### cm "webapp-color"
```
Kubectl describe configmaps webapp-color		
```
```
kubectl edit configmap CONFIGMAP_NAME
```

### to get the format to update yaml file.
```
kubectl explain pods --recursive | grep envFrom -A3		
```
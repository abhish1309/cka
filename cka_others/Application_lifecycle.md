# *Application Lifecyle management* :-


### to check the status of roll out
```
kubectl rollout status deployment/myapp-deployment		
```

### to check history
```
kubectl rollout history deployment/myapp-deployment		
```

### here the deployment.yaml file will not be changed, so different configuration will be there in current running pod and the file.
```
kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1		
```

### if update wont work, we can reverse the update.
```
kubectl rollout undo deployment/myapp-deployment		
```

### check command option.
```
kubectl describe pod ubuntu-sleeper		
```
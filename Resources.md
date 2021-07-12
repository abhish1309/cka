# *ResourceQuota* :-

### to create quota
```
kubectl create quota -h
```

### create pod with limited resources
```
kubectl run mypod --image=nginx --restart=Never \
--requests='cpu=250m,memory=64Mi' \
--limits='cpu=520m,memory=128'
```

### create quota
```
kubectl create quota myrq --hard=cpu=1,memory=1G,pods=2
```

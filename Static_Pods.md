# *Static Pods* :-

### to check static pods creation. because its indipendent from cluster.
```
docker ps		
```

### kubelete is responsible to run static pods
```
ps -aux | grep kubelet		
```

### copy the path of --config=/..../config.yaml
```
ps -aux | grep kubelet | grep .yaml		
```

### get the path of pod files location.
```
cat /..../config.yaml | grep -i staticPodPath		
```

---
### contains .yaml file // search --config on kubelet.
```
ps -aux | grep -i "\--config"		
```

### kubelet config file location; here the static pod definition file location is configured.
```
cat /var/lib/kubelet/config.yaml | grep -i staticpod		
```

### to list the files of default static pod definitions present in manifest folder.
```
ls /etc/kubernetes/manifests		
```
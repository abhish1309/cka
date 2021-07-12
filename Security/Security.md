# *Security* :-

### user generate the key
```
openssl gerrsa -out my-bank.key 1024			
```

### user generate the CertificateSignRequest to send to admin
```
openssl rsa -in mybank.key -pubout > mybank.pem		
```

### to generate a certificateSignRequest
```
opensll req -new -key my-bank.key -out my-bank.scr -subj "/C=US/ST-CA/0=MyOrg, Inc./CN=my-bank.com"		
```

### Deployed Kubernetes from Scratch - the hard way
```
cat /etc/systemd/system/kube-apiserver.service			
```

### Deployed Kubernetes via kubeadm
```
cat /etc/kubernetes/manifests/kube-apiserver.yaml	
```


### to open and check the certificate details.
```
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -txt -noout		
```
### if from scratch; check logs with the cmd
```
journalctl -u etcd.service -l			
```

### if from kubeadm; logs followed by pod name (kube-system)
```
kubectl logs etcd-master			
```

### if not found via kubernetes; check one level down from docker, get the container id.
```
dcoker ps -a			
```

### check the logs of 87fc cont id.
```
docker logs 87fc		
```
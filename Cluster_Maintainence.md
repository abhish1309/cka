# *Cluster Maintenance* :-


## *Join Node*

### print the link used to join the node to cluster.
kubeadm token create --print-join-command		


## *Os Upgrades*

### it simply marks the node as "unschedulable" and do not terminates the existing pods; it makes sure the new pods are not created on it.
kubectl conrdon node-2		

### existing pods are terminated from the "node-1" and recreated on another node; and "node-1" is marked as cordan and unschedulable.
kubectl drain node-1		

### node-1 marked as uncordan to "schedule" pods.
kubectl uncordon node-1		

### We cannot delete/drain DaemonSet-managed pods; so we ingnored it.
kubectl drain node01 --ignore-daemonsets		



## *Kubernetes Software Versions* :-
### to check the version
kubeadm version


## *Cluster upgrade* :-
### to check the cluster info
kubectl cluster-info

### to check the current version
kubeadm version		

### to check the version in short
kubectl version --short		

### to check upgrade available
kubeadm upgrade plan		


## *Upgrade master node* :-
### to upgrade kubeadm & kublete to perticular versions 

kubectl drain controlplane --ignore-daemonsets

apt install kubeadm=1.18.0-00

kubeadm upgrade apply v1.18.0

apt install kubelet=1.18.0-00

kubectl uncordon controlplane



## *Upgrade worker node* :-

## from controlplane terminal.
kubectl drain node01		

ssh node01

apt install kubeadm=1.18.0-00

kubeadm upgrade node

apt install kubelet=1.18.0-00

exit

### from controlplane terminal
kubectl uncordon node01		


## *Backup and restore* :-
### to backup manually to the files.
kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml

 
### to get the backup of etcd.
### ### remember to specify --endpoints=" ",--cacert,--cert,--key  ; if running cmd on etcd which is on same master then no need apply endpoints.snapshotname: snapshot01.db; it is saved in pwd; specify path if required.
```
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
snapshot save /opt/snapshot-pre-boot.db 		
```

### snapshot.db ; files in pwd		### Backup using builtin snapshot utility
ls		

### check status
ETCDCTL_API=3 etcdctl snapshot status snapshot01.db		
  
###  stop kube-apiserver
service kube-apiserver stop		

### to restore ETCD cluster;	
ETCDCTL_API=3 etcdctl snapshot restore snapshot01.db --data-dir /var/lib/etcd-from-backup		

  * ### locate to manifest file location 
  cd /etc/kubernetes/manifests/		 
  
  * ### volumes >hostPath >path:/varlib/etcd-from-backup (its mandatory)
  vi etcd.yaml 	

  * ### reload restart daemon
  systemctl daemon-reload

  * ### restart etcd 
  service etcd restart

  * ### start the kube-apiserver
  service kube-apiserver start

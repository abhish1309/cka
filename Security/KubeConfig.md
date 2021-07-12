# *kubeconfig* :-

### to check the no of clusters present.
kubectl config view   

### to check context
kubectl config view --kubeconfig=my-custom-config  

### to change context to production.
kubectl config use-context prod-user@production   

### list of supported commads
kubectl config  -h

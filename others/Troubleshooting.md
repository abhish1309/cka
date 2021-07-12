# *Troubleshooting* :-

1) check connectivity

>curl http://web-service-ip:node-port

>curl http://172.16.10.10:8181

2) check if pod is in running state
>kubectl get pod

3) check logs 
>kubectl logs web -f
>kubectl logs web -f --previous

4) check endpoints in namespace
>kubectl -n gama get ep

5) check environment variables.



# *Worker node failure* :-

## check if its ready
>kubectl get nodes			

## check if the status is true:-not healthy due to symtoms, false: healty
>kubectl describe node worker-1		

>top
>df -h 

>serive kubelet status
>sudo journalctl -u kubelet

>openssl x509 -in /var/lib/kubelet/worker-1.crt -text

1) check if the process for kubelet is running.
>ps -ef | grep -i kubelet		
>systemctl status kubelet.service
>systemctl restart kubelet

2) certificate issue
>sudo journalctl -u kubelet
>cd /etc/systemd/system/kubelet.service.d/
>cat 10-kubeadm.conf

>cat /../kubelet.yaml
	- check the certificate 
	- correct the certificate url

>systemctl daemon reload
>systemctl restart kubelet

3)
>cat 10-kubeadm.conf
	--kubeconfig=/etc/kubernetes/kubelet.conf


#6553 to 6443 port
>vi /etc/kubernetes/kubelet.conf
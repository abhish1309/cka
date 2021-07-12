# *Cron Jobs* :-

### create cron jobs
```
kubectl create cronjob busybox --image=busybox \
--schedule="*/1 * * * *" \
-- bin/sh -c 'date; echo Hello from the kubernetes cluster'
```
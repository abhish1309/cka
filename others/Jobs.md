## *Jobs* :-

### create job
```
kubectl create job math-add-job --image=ubuntu -- expr 3 + 2
```

### get jobs details
```
kubectl get jobs
```

### check logs of jobs
```
kubectl logs job math-add-job-* 
```

### delete the job
```
kubectl delete job match-add-job-*
```
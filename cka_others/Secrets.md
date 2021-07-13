# *Secrets* :-

## *Generic*

### imperative approach, to create secret directly from command; entering direct parameters
```
kubectl create secret generic <secret-name> --from-literal=<key>=<value>		
```
```
kubectl create secret generic app-secret \
--from-literal=DB_Host=mysql \
--from-literal=DB_User=root \
--from-literal=DB_Password=paswrd
```

### imperative approach using file
```
kubectl create secret generic <secret-name> --from-file=<pathToFile>			
```

### file "app_secret.properties"
```
kubectl create secret gereric app-secret --from-file=app_secret.properties		
```

### declarative approach, to create a secret from yaml file
```
kubectl create -f secret-data.yaml		
```

### to get list of secrets
```
kubectl get secrets		
```

### to describe newly created secret; without displaying value
```
kubectl describe secrets		
```

### shows the yaml file; with displaying values
```
kubectl describe secrets -o yaml		
```

### to edit the secret
```
kubectl edit secret SECRET_NAME		
```

### encoding value; to convert normal text "mysql" to encoded text "bX1zcWw="; to save encoded text in secret file instead of normal plain text.
```
echo -n 'mysql' | base64		
```

### decoding; to convert encoded text 'bx1zcWw=' to normal plain text 'mysql'
```
echo -n 'bx1zcWw=' | base64 --decode		
```

## *Docker-registry*

### create secret docker-registry 
```
kubectl create secret docker-registry regcred \
--docker-server=<your-registry-server> \
--docker-username=<your-name> \
--docker-password=<your-pword> \
--docker-email=<your-email>
```
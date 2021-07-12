# *CertificatesSignRequest* :-

### user generates the key
```
openssl genrsa -out jane.key 2048		
```

### user then create certificateSignRequest with the name on it and sends to admin
```
openssl req -new -key jane.key -subj "/CN=jane" -out jane.csr		
```

### convert value to base64 to use under request feild of yaml file( CSR object)
```
cat myuser.csr | base64 | tr -d "\n"		

```

### administrator takes a key and generates certificateSignRequest object (like other kubernetes object) with a manifest file.



#### Get list of CSRs
```
kubectl get csr		
```

### approve the CSR
```
kubectl certificate approve mysuser		
```

### Retrieve the certificate from the CSR; and manualy export from base64 ; select from certificate:   ;or else follow below cmd.
```
kubectl get csr/myuser -o yaml		
```


### The certificate value is in Base64-encoded format under status.certificate. Export the issued certificate from the CertificateSigningRequest.
```
kubectl get csr myuser -o jsonpath='{.status.certificate}'| base64 -d > myuser.crt		
```



# Base64
```
$ username=$(echo -n "admin" | base64)
$ password=$(echo -n "a62fjbd37942dcs" | base64)
```
Put `username` and `password` to the `secret.yaml`
```
$ echo $username
YWRtaW4=
$ echo $password
YTYyZmpiZDM3OTQyZGNz
```

# Create secret
```bash
$ kubectl create -f secret.yaml
secret/test-secret created
$ kubectl get secrets
NAME                  TYPE                                  DATA   AGE
default-token-vnngd   kubernetes.io/service-account-token   3      2d
test-secret           Opaque                                2      6s
```

# Check the secret
```bash
kubectl exec -it secret-env-pod env | grep SECRET_
SECRET_USERNAME=admin
SECRET_PASSWORD=a62fjbd37942dcs
```

# To mount secret as volumes
To mount the secrets as volumes we first define a volume with a well-known name, 
in this case, secret-volume, and provide it with our stored secret.

```bash
volumes:
 - name: secret-volume
   secret:
     secretName: test-secret
```

When we define the container we mount our created volume to a particular directory. 
Applications will read the secrets as files from this path.

```bash
volumeMounts:
 - name: secret-volume
   mountPath: /etc/secret-volume
```


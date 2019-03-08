

# Kompose
Make `docker-compose`
`kompose up`
```bash
$ kubectl get deployment,svc,pods,pvc
NAME                                 DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/frontend       1         1         1            1           28s
deployment.extensions/redis-master   1         1         1            1           28s
deployment.extensions/redis-slave    1         1         1            1           28s

NAME                   TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/frontend       ClusterIP   10.98.157.97     <none>        80/TCP     29s
service/kubernetes     ClusterIP   10.96.0.1        <none>        443/TCP    21m
service/redis-master   ClusterIP   10.104.112.74    <none>        6379/TCP   29s
service/redis-slave    ClusterIP   10.111.193.110   <none>        6379/TCP   29s

NAME                                READY     STATUS    RESTARTS   AGE
pod/frontend-7c548dc557-cpj9q       1/1       Running   0          28s
pod/redis-master-6574c44848-8dnch   1/1       Running   0          28s
pod/redis-slave-68b45d6b86-nj2kz    1/1       Running   0          28s
```

Get manifest files
`$ kompose convert -f docker-compose.yml` or short-hand `kompose convert`

`Volume and network` is not fully support yet.
[Details](https://github.com/kubernetes/kompose)

# Reference:
https://www.katacoda.com/courses/kubernetes/deploy-docker-compose-using-kompose
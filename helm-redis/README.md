# Helm Usage
```bash
$ helm search redis
NAME                            	CHART VERSION	APP VERSION	DESCRIPTION
stable/prometheus-redis-exporter	1.0.2        	0.28.0     	Prometheus exporter for Redis metrics
stable/redis                    	6.1.4        	4.0.13     	Open source, advanced key-value store. It is often referr...
stable/redis-ha                 	3.3.0        	5.0.3      	Highly available Kubernetes implementation of Redis
stable/sensu                    	0.2.3        	0.28       	Sensu monitoring framework backed by the Redis transport
```

Install
```bash
$ helm install --name elcolie stable/redis
NAME:   plinking-narwhal
LAST DEPLOYED: Thu Mar  7 04:50:00 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                           DATA  AGE
plinking-narwhal-redis         3     0s
plinking-narwhal-redis-health  3     0s

==> v1/Service
NAME                           TYPE       CLUSTER-IP      EXTERNAL-IP  PORT(S)   AGE
plinking-narwhal-redis-master  ClusterIP  10.109.186.189  <none>       6379/TCP  0s
plinking-narwhal-redis-slave   ClusterIP  10.99.122.12    <none>       6379/TCP  0s

==> v1beta1/Deployment
NAME                          DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
plinking-narwhal-redis-slave  1        1        1           0          0s

==> v1beta2/StatefulSet
NAME                           DESIRED  CURRENT  AGE
plinking-narwhal-redis-master  1        1        0s

==> v1/Pod(related)
NAME                                          READY  STATUS             RESTARTS  AGE
plinking-narwhal-redis-slave-9b645b597-2vh82  0/1    ContainerCreating  0         0s
plinking-narwhal-redis-master-0               0/1    Pending            0         0s

==> v1/Secret
NAME                    TYPE    DATA  AGE
plinking-narwhal-redis  Opaque  1     0s


NOTES:
** Please be patient while the chart is being deployed **
Redis can be accessed via port 6379 on the following DNS names from within your cluster:

plinking-narwhal-redis-master.default.svc.cluster.local for read/write operations
plinking-narwhal-redis-slave.default.svc.cluster.local for read-only operations


To get your password run:

    export REDIS_PASSWORD=$(kubectl get secret --namespace default plinking-narwhal-redis -o jsonpath="{.data.redis-password}" | base64 --decode)

To connect to your Redis server:

1. Run a Redis pod that you can use as a client:

   kubectl run --namespace default plinking-narwhal-redis-client --rm --tty -i --restart='Never' \
    --env REDIS_PASSWORD=$REDIS_PASSWORD \
   --image docker.io/bitnami/redis:4.0.13 -- bash

2. Connect using the Redis CLI:
   redis-cli -h plinking-narwhal-redis-master -a $REDIS_PASSWORD
   redis-cli -h plinking-narwhal-redis-slave -a $REDIS_PASSWORD

To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace default svc/plinking-narwhal-redis 6379:6379 &
    redis-cli -h 127.0.0.1 -p 6379 -a $REDIS_PASSWORD

```

See what `helm` is running
```bash
$ helm ls
NAME                    REVISION        UPDATED                         STATUS          CHART           NAMESPACE
plinking-narwhal        1               Thu Mar  7 04:50:00 2019        DEPLOYED        redis-6.1.4     default
```

Explore our cluster after running `redis` system
```bash
$ kubectl get all
NAME                                               READY     STATUS    RESTARTS   AGE
pod/plinking-narwhal-redis-master-0                0/1       Pending   0          1m
pod/plinking-narwhal-redis-slave-9b645b597-2vh82   0/1       Running   2          1m

NAME                                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/kubernetes                      ClusterIP   10.96.0.1        <none>        443/TCP    5m
service/plinking-narwhal-redis-master   ClusterIP   10.109.186.189   <none>        6379/TCP   1m
service/plinking-narwhal-redis-slave    ClusterIP   10.99.122.12     <none>        6379/TCP   1m

NAME                                           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/plinking-narwhal-redis-slave   1         1         1            0           1m

NAME                                                     DESIRED   CURRENT   READY     AGE
replicaset.apps/plinking-narwhal-redis-slave-9b645b597   1         1         0         1m

NAME                                             DESIRED   CURRENT   AGE
statefulset.apps/plinking-narwhal-redis-master   1         1         1m
```

Pod will be in `pending` state untile `Persisten Volume` is available
```bash
kubectl apply -f pv.yaml
```

Redis needs permissions to write
They are `data1, data2, and data3` according to `pv.yaml`
```bash
chmod 777 -R /mnt/data*
```



https://www.katacoda.com/courses/kubernetes/helm-package-manager
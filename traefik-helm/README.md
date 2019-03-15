```bash
$ helm install --name elfik stable/traefik
NAME:   elfik
LAST DEPLOYED: Fri Mar  8 15:36:59 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME           DATA  AGE
elfik-traefik  1     1s

==> v1/Deployment
NAME           READY  UP-TO-DATE  AVAILABLE  AGE
elfik-traefik  0/1    1           0          1s

==> v1/Pod(related)
NAME                            READY  STATUS             RESTARTS  AGE
elfik-traefik-69dd5cbbbc-62lhv  0/1    ContainerCreating  0         0s

==> v1/Service
NAME           TYPE          CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
elfik-traefik  LoadBalancer  10.51.252.9  <pending>    80:32636/TCP,443:30615/TCP  1s


NOTES:

1. Get Traefik's load balancer IP/hostname:

     NOTE: It may take a few minutes for this to become available.

     You can watch the status by running:

         $ kubectl get svc elfik-traefik --namespace default -w

     Once 'EXTERNAL-IP' is no longer '<pending>':

         $ kubectl describe svc elfik-traefik --namespace default | grep Ingress | awk '{print $3}'

2. Configure DNS records corresponding to Kubernetes ingress resources to point to the load balancer IP/hostname found in step 1
```

To get a manifest file
`$ helm install --dry-run --debug stable/traefik`
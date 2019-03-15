# Note
```bash
$ helm install stable/nginx-ingress --name quickstart
NAME:   quickstart
LAST DEPLOYED: Mon Mar 11 14:08:00 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                 DATA  AGE
quickstart-nginx-ingress-controller  1     0s

==> v1/Pod(related)
NAME                                                       READY  STATUS             RESTARTS  AGE
quickstart-nginx-ingress-controller-56bb9865c9-rm7qq       0/1    ContainerCreating  0         0s
quickstart-nginx-ingress-default-backend-57bdfdcd46-d4h2n  0/1    ContainerCreating  0         0s

==> v1/Service
NAME                                      TYPE          CLUSTER-IP     EXTERNAL-IP  PORT(S)                     AGE
quickstart-nginx-ingress-controller       LoadBalancer  10.51.246.152  <pending>    80:32654/TCP,443:31180/TCP  0s
quickstart-nginx-ingress-default-backend  ClusterIP     10.51.252.114  <none>       80/TCP                      0s

==> v1/ServiceAccount
NAME                      SECRETS  AGE
quickstart-nginx-ingress  1        0s

==> v1beta1/ClusterRole
NAME                      AGE
quickstart-nginx-ingress  1s

==> v1beta1/ClusterRoleBinding
NAME                      AGE
quickstart-nginx-ingress  1s

==> v1beta1/Deployment
NAME                                      READY  UP-TO-DATE  AVAILABLE  AGE
quickstart-nginx-ingress-controller       0/1    1           0          1s
quickstart-nginx-ingress-default-backend  0/1    1           0          1s

==> v1beta1/Role
NAME                      AGE
quickstart-nginx-ingress  1s

==> v1beta1/RoleBinding
NAME                      AGE
quickstart-nginx-ingress  1s


NOTES:
The nginx-ingress controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace default get services -o wide -w quickstart-nginx-ingress-controller'

An example Ingress that makes use of the controller:

  apiVersion: extensions/v1beta1
  kind: Ingress
  metadata:
    annotations:
      kubernetes.io/ingress.class: nginx
    name: example
    namespace: foo
  spec:
    rules:
      - host: www.example.com
        http:
          paths:
            - backend:
                serviceName: exampleService
                servicePort: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
        - hosts:
            - www.example.com
          secretName: example-tls

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
```

# Quirk
The IP address on the ingress may not match the IP address that the nginx-ingress-controller. This is fine, and is a quirk/implementation detail of the service provider hosting your Kubernetes cluster. Since we are using the nginx-ingress-controller instead of any cloud-provider specific ingress backend, use the IP address that was defined and allocated for the nginx-ingress-service LoadBalancer resource as the primary access point for your service.
```bash
$ kubectl get ing,svc
NAME                       HOSTS            ADDRESS         PORTS     AGE
ingress.extensions/kuard   singh.hbot.dev   35.198.217.71   80, 443   14s

NAME                                               TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)                      AGE
service/kuard                                      ClusterIP      10.51.255.3     <none>           80/TCP                       35s
service/kubernetes                                 ClusterIP      10.51.240.1     <none>           443/TCP                      9d
service/quickstart-nginx-ingress-controller        LoadBalancer   10.51.252.239   35.197.155.131   80:30355/TCP,443:32366/TCP   6m9s
service/quickstart-nginx-ingress-default-backend   ClusterIP      10.51.251.148   <none>           80/TCP                       6m8s
```

# Cert-Manager
File is very long. Then download from the Internet
`kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.7/deploy/manifests/00-crds.yaml`

# helm delete
`helm list --all`
`helm del pruge quickstart`

# Reference
https://github.com/jetstack/cert-manager/blob/master/docs/tutorials/acme/quick-start/index.rst




$ kubectl get ingress --all-namespaces
NAMESPACE   NAME    HOSTS            ADDRESS         PORTS     AGE
default     kuard   singh.hbot.dev   35.198.217.71   80, 443   43m
$ kubectl describe ingress
Name:             kuard
Namespace:        default
Address:          35.198.217.71
Default backend:  default-http-backend:80 (10.48.0.7:8080)
TLS:
  quickstart-example-tls terminates singh.hbot.dev
Rules:
  Host            Path  Backends
  ----            ----  --------
  singh.hbot.dev
                  /   kuard:80 (<none>)
Annotations:
  certmanager.k8s.io/acme-challenge-type:            http01
  certmanager.k8s.io/issuer:                         letsencrypt-prod
  kubectl.kubernetes.io/last-applied-configuration:  {"apiVersion":"extensions/v1beta1","kind":"Ingress","metadata":{"annotations":{"certmanager.k8s.io/acme-challenge-type":"http01","certmanager.k8s.io/issuer":"letsencrypt-prod","kubernetes.io/ingress.class":"nginx"},"name":"kuard","namespace":"default"},"spec":{"rules":[{"host":"singh.hbot.dev","http":{"paths":[{"backend":{"serviceName":"kuard","servicePort":80},"path":"/"}]}}],"tls":[{"hosts":["singh.hbot.dev"],"secretName":"quickstart-example-tls"}]}}

  kubernetes.io/ingress.class:  nginx
Events:
  Type    Reason             Age                From                      Message
  ----    ------             ----               ----                      -------
  Normal  CREATE             43m                nginx-ingress-controller  Ingress default/kuard
  Normal  CreateCertificate  43m                cert-manager              Successfully created Certificate "quickstart-example-tls"
  Normal  UPDATE             10m (x2 over 43m)  nginx-ingress-controller  Ingress default/kuard
  Normal  UpdateCertificate  10m                cert-manager              Successfully updated Certificate "quickstart-example-tls"
# Difficulty: beginner
Kubernetes have advanced networking capabilities that allow Pods and Services to communicate inside the cluster's network. An Ingress enables inbound connections to the cluster, allowing external traffic to reach the correct Pod.

Ingress enables externally-reachable urls, load balance traffic, terminate SSL, offer name based virtual hosting for a Kubernetes cluster.

In this scenario you will learn how to deploy and configure Ingress rules to manage incoming HTTP requests.


```bash
kubectl get ingress --all-namespaces
NAMESPACE   NAME             HOSTS           ADDRESS        PORTS   AGE
default     webapp-ingress   singh.hbot.io   34.95.66.133   80      5m
```

WARNING: DO NOT USE MASTER IP. YOU MUST MUST THE IP FROM THE CLI ONLY
Then use the given IP with DNS
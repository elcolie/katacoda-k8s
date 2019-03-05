HBot Bangwa
===========


# Prerequisite
1. Extract Google SDK to project root directory
1. `./google-cloud-sdk/install.sh`
1. `./google-cloud-sdk/bin/gcloud init`

# Example when using GSDK
1. `gcloud config list project`
1. `gcloud container clusters --zone=asia-southeast1-a get-credentials platform`
1. `gcloud container clusters create singh --num-nodes=3  --machine-type n1-standard-1 --zone=asia-southeast1-a` Make new cluster
1. `gcloud container clusters --zone asia-southeast1-a get-credentials singh` Change working cluster to `singh` cluster
1. `kubectl get nodes` To see the working nodes in the cluster
    ```
    NAME                                   STATUS   ROLES    AGE   VERSION
    gke-singh-default-pool-c860aa65-0188   Ready    <none>   19m   v1.11.6-gke.2
    gke-singh-default-pool-c860aa65-gnsn   Ready    <none>   19m   v1.11.6-gke.2
    gke-singh-default-pool-c860aa65-vl5x   Ready    <none>   19m   v1.11.6-gke.2
    ```

1. Create `service account` on cluster
    ```
    kubectl -n kube-system create sa tiller
    ```
    
1. Bind clusteradmin role to `tiller`
    ```bash
    kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
    ``` 
    
1. Init helm on cluster with tiller
   ```
    helm init --service-account tiller
   ```

1. `helm install stable/mongodb` Install mongodb stable version
1. `kubectl get secret odd-monkey-mongodb -o yaml` Check the secret given by `tiller`

1. `helm list`
    ```javascript 1.5
    NAME      	REVISION	UPDATED                 	STATUS  	CHART        	APP VERSION	NAMESPACE
    odd-monkey	1       	Mon Feb 18 13:32:20 2019	DEPLOYED	mongodb-5.3.4	4.0.6      	default
    ```
1. `helm delete odd-monkey`
    ```
    release "odd-monkey" deleted
    ```
    
# Miscellaneous
1. `gcloud config list project`
1. `gcloud container clusters get-credentials platform`

1. `gcloud container clusters --zone asia-southeast1-a get-credentials platform`
    ```javascript 1.5
    >Fetching cluster endpoint and auth data.
    >kubeconfig entry generated for platform.
    ```

1. `kubectl config view`
    ```javascript 1.5
    > แสดงว่า `current-context` คือ cluster ไหน ต้องระวัง
    ```

1. `kubectl config set-context gke_hbot-bangwa_asia-southeast1-a_singh`
    > เป็นการเปลี่ยน context

1. `kubectl get secret odd-monkey-mongodb -o yaml`


Resources
---------

- jira : https://unicornonzen.atlassian.net/browse/BAN
- repository : git.unicornonzen.com
- gcp project : hbot-bangwa
- firebase project : bot-platform-dev
- container registry : asia.gcr.io
- cicd : https://app.buddy.works
- cloudflare : https://dash.cloudflare.com/fe1236fd490564925d88e80bbe8de71e
- facebookapp : https://developers.facebook.com/apps/2217545521807140/webhooks/

- TLS: https://blog.n1analytics.com/free-automated-tls-certificates-on-k8s/
- Helm: https://helm.sh/docs/using_helm/#role-based-access-control

- https://cloud.google.com/kubernetes-engine/docs/tutorials/http-balancer

# Error
https://stackoverflow.com/questions/46672523/helm-list-cannot-list-configmaps-in-the-namespace-kube-system

helm list
Error: configmaps is forbidden: User "system:serviceaccount:kube-system:default" cannot list configmaps in the namespace "kube-system"

kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'      
helm init --service-account tiller --upgrade


# Give myself admin access
```bash
kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole cluster-admin \
  --user $(gcloud config get-value account)
```

# Jump to pods
```bash
$ kubectl get po -n nginx-ingress
NAME                             READY   STATUS    RESTARTS   AGE
nginx-ingress-77fcd48f4d-l6mvr   1/1     Running   0          4m

$ kubectl exec -it nginx-ingress-77fcd48f4d-l6mvr -n nginx-ingress /bin/bash
root@nginx-ingress-77fcd48f4d-l6mvr:/#
```


# ps command not found
`apt-get update && apt-get install -y procps`

# IP
INTERNAL-IP   EXTERNAL-IP   
10.148.0.46   35.197.128.107
10.148.0.47   35.198.217.71 
10.148.0.45   35.197.159.75 

# Recommend
`Browser -> Load Balancer -> nginx ingress -> pods`
Using this method, all the ingress you will create will get the same IP, the IP of the load balancer.
and the nginx ingress is what will route the request to the right pods.


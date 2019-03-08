# Deployment from source
https://www.katacoda.com/courses/kubernetes/deploy-service-from-source

# Build image
`docker build -t hello-webapp:v1 .`

# Run image
`docker run -d -p 80:8080 hello-webapp:v1`

# Tag
```bash
export REGISTRY=2886795420-5000-kitek05.environments.katacoda.com
docker tag hello-webapp:v1 $REGISTRY/hello-webapp:v1
docker push $REGISTRY/hello-webapp:v1
```

# PORT
```bash
export PORT=$(kubectl get svc hello-webapp -o go-template='{{range.spec.ports}}{{if .nodePort}}{{.nodePort}}{{"\n"}}{{end}}{{end}}')
```

# Test
```bash
curl localhost:$PORT
```

# forge
Cool tool
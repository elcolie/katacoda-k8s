# Traefik
https://www.katacoda.com/courses/traefik/deploy-load-balancer

# Imperative style scaling
`docker-compose up --scale machine=2`

# See loadbalancer in action
```bash
$ curl -H Host:machine-echo.example.com http://localhost
$ curl -H Host:machine-echo.example.com http://localhost
<h1>This request was processed by host: 940c09c2d039</h1>
$ curl -H Host:machine-echo.example.com http://localhost
<h1>This request was processed by host: 6a53034c43e8</h1> 
```

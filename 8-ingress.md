# Introduction

Now we got our service running in the cluster, we can curl it using the service name and the port inside a one-of running pod

```
$ k run -it --rm --restart=Never testing --image=cfmanteiga/alpine-bash-curl-jq bash
$ curl http://kubernetes-bootcamp:8080
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-6d64485df4-dbpnz | v=1
```

Now we need to make the application available to the outer world. 
We need to tell Kubernetes to route HTTP requests from the outside to a certain location to this service.

This process is called [ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/).

# Tasks

## 1. Ingress controller
The ingress controller is a reverse proxy running within Kubernetes.
so we need to enable the extension inside `minikube`

```
$ minikube addons enable ingress
✅  ingress was successfully enabled
```

Please take a look at [8-ingress.yaml](8-ingress.yaml) for the description of our sample ingress.

### Challenges

* Apply the ingress to the cluster
* Find the exposed IP-address and invoke it (hint; use `minikube` command or dashboard)
* Explore the ingress in the Kubernetes Dashboard
* Use the `k describe ingress` to find the path and backends for your ingress


## Add second deploy and add it to the ingress

```
$ k run web2 --image=gcr.io/google-samples/hello-app:2.0 --port=8080
$ k expose deployment web2 --target-port=8080 --type=NodePort
```

### Challenges 
* add this deploy to the ingress.yaml and apply it
* find the two different ``curl`` commands to run to hit the two different deploys
* test and open the addresses in the browser

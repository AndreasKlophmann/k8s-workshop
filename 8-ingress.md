# Introduction

Now we got our service running in the cluster, we can curl it using the service name and the port inside a one-of running pod.

```
$ k run -it --rm --restart=Never testing --image=cfmanteiga/alpine-bash-curl-jq bash
$ curl http://k8s-workshop:8080
Hello Kubernetes bootcamp! | Running on: k8s-workshop-6d64485df4-dbpnz | v=1
```

Now we need to make the application available to the outer world. 
We need to tell Kubernetes to route HTTP requests from the outside to a certain location to this service.

This process is called [ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/).

### What is Ingress?
Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. 
Traffic routing is controlled by rules defined on the Ingress resource.

An Ingress can be configured to give Services externally-reachable URLs, load balance traffic, terminate SSL / TLS, 
and offer name based virtual hosting. An Ingress controller is responsible for fulfilling the Ingress, usually with a load balancer, 
though it may also configure your edge router or additional frontends to help handle the traffic.

An Ingress does not expose arbitrary ports or protocols. Exposing services other than HTTP and HTTPS to the internet 
typically uses a service of type Service.Type=NodePort or Service.Type=LoadBalancer

# Tasks

## 1. Install the ingress controller
The ingress controller is a reverse proxy running within Kubernetes.
so we need to enable the extension inside `minikube`

```
$ minikube addons enable ingress
✅  ingress was successfully enabled
```

Please take a look at [8-ingress.yaml](8-ingress.yaml) for the description of our sample ingress.

### Challenges

* Apply the ingress to the cluster
* Find the exposed IP-address and open it in the browser (hint: use kubectl to get or describe the ingress)
* Explore the ingress in the Kubernetes Dashboard
* Use the `k describe ingress` to find the path and backends for your ingress


## Add second deploy and add it to the ingress

```
$ k run web2 --image=gcr.io/google-samples/hello-app:2.0 --port=8080
$ k expose deployment web2 --target-port=8080
```

### Challenges 
* add this deploy to the ingress.yaml under a different path and apply it
* find the two different ``curl`` commands to run to hit the two different deploys

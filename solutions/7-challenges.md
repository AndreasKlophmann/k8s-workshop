### Invoke our application using the new service endpoint.
```
$ k get services
NAME                  TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)    AGE
kubernetes            ClusterIP   10.96.0.1     <none>        443/TCP    26m
kubernetes-bootcamp   ClusterIP   10.96.18.10   <none>        8080/TCP   9m25s
$ k run -it --rm --restart=Never testing --image=cfmanteiga/alpine-bash-curl-jq bash
$ curl http://10.96.18.10:8080/proxy
```

### What is the IP? Port?
```
k get services
NAME                  TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)    AGE
kubernetes            ClusterIP   10.96.0.1     <none>        443/TCP    26m
kubernetes-bootcamp   ClusterIP   10.96.18.10   <none>        8080/TCP   9m25s
```

### Use the dashboard and see which resources are active in our cluster
```
$ minikube dashboard
```


### Scale our deployment to 2 pods (hint: see `7-kubernetes-bootcamp.yaml`)
- Change number of replicas to 2
```
$ k get pods -w -o wide # another terminal
$ k apply -f 7-kubernetes-bootcamp.yaml
```

### With the logs of both pods open in separated windows, invoke the service IP and observe what happens
```
$ k get pods
$ k logs -f <pod-name> # two different terminals
$ k run -it --rm --restart=Never testing --image=cfmanteiga/alpine-bash-curl-jq bash
$ curl http://10.96.18.10:8080/proxy
```


### Start a one-off pod with bash and curl and try to invoke our service using DNS instead of virtual IP (hint: DNS name = service name)
```
$ k run -it --rm --restart=Never testing --image=cfmanteiga/alpine-bash-curl-jq bash
$ curl http://kubernetes-bootcamp:8080/proxy
```
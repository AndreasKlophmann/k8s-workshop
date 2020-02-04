# Introduction

In the previous sections we have learned how Deployments control how pods are created for our application and that it even defines how many replicas we should have and how pods are distributed on our nodes. But we still need to solve how we're goning to route traffic to our app and load balance between the replicas. And for that we need Services.

# Tasks

## 1. Service

[Kubernetes Services](https://kubernetes.io/docs/concepts/services-networking/service/) is an abstraction that allows loose coupling of pods to enable load balancing, discovery and routing. We can expose our application as a service using `kubectl expose`:

```
$ k expose deploy kubernetes-bootcamp
```

We could also expose our service using the deployment descriptor from section 4:

```
$ k expose -f 4-kubernetes-bootcamp.yaml
```

### Challenges

* Invoke our application using the new service endpoint.
* What is the IP? Port?
* Use the dashboard and see which resources are active in our cluster

## 2. Load balancing

The IP-address listed next to our service is a special kind of address. It is a virtual IP-address. It doesn't point to one specific pod but to a service that can consist of multiple pods. This way we can achieve both load balancing and high availability!

```
$ k get services
NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
kubernetes            ClusterIP   10.96.0.1      <none>        443/TCP    7d9h
kubernetes-bootcamp   ClusterIP   10.96.80.156   <none>        8080/TCP   159m
```

We can access the virtual IP either from a pod inside the cluster or using the `kubectl proxy` command.

```
$ k proxy
$ curl http://localhost:8001/api/v1/namespaces/default/services/kubernetes-bootcamp/proxy/
```

Or alternatively from inside the cluster using the minikube pod:

```
$ minikube ssh
$ curl http://10.96.80.156:8080
```

### Challenges

* Scale our deployment to 2 pods (hint: see `7-kubernetes-bootcamp.yaml`)
* With the logs of both pods open in separated windows, invoke the service IP and observe what happens

## 3. Manifest files

We have briefly looked at manifest files which is the declarative way to interface with Kubernetes (kubectl being the command line interface). This is the preferred way to build, store and share kubernetes setup and adheres to the principle of infrastructure as code.

A manifest file (yaml or json) can contain multiple resources of different kinds together with metadata and spec. The different entries must be separated by triple dash separator which signifies the start of a new document in yaml.

The resource specifications are documented in great detail in the [Kubernetes API documentation](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.17/). In addtion the `kubectl` command is self documenting using `kubectl explain`:

```
$ k explain pods
KIND:     Pod
VERSION:  v1

DESCRIPTION:
     Pod is a collection of containers that can run on a host. This resource is
     created by clients and scheduled onto hosts.

FIELDS:
   apiVersion	<string>
...
```

We can also drill down into more details:

```
$ k explain pods.spec
KIND:     Pod
VERSION:  v1

RESOURCE: spec <Object>

DESCRIPTION:
     Specification of the desired behavior of the pod. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

     PodSpec is a description of a pod.
...
``` 

## 4. Selectors

If we take a look at the sample manifest file in `7-kubernetes-bootcamp.yaml` we can see that the only thing connecting the service definition to deployment definition is the line below `selectors` in the service which is identical to the first line under `labels` in the deployment:

```
labels:
  app: kubernetes-bootcamp
---
selector:
  app: kubernetes-bootcamp
```

The service selector tells kubernetes which pods to include in the service. This selector-based mechanism is used by many components in Kubernetes, and it is very versatile in that it allows custom labels. This opens up a whole lot of possibilities for different patterns, such as A/B deployments, rolling updates (which we will see later) and similar things.

You can use selectors to filter the results in many `kubectl` commands using the `--selector=` og just `-l`:

```
$ k get pods -l app=kubernetes-bootcamp
```

### Challenges

* Start a one-off pod with bash and curl and try to invoce our service using DNS instead of virtual IP (hint: DNS name = service name)
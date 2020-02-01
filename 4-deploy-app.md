# Introduction

To better understand how Kubernetes works we want to install a "real" app into our minikube cluster. Then we can check how it behaves and what is possible to do with it.

We can also fetch and deploy Docker container images quite easy, this is often used to for example curl services.

!! Make sure you are in the correct minikube context before starting this step.
```
$ k config current-context
minikube
```


# Tasks

## 1. Configure Docker
To make it easier for you to play around with minikube, kuberenetes and docker we will configure the Docker client to be directed at the deamon running inside minikube, instead of the local one. You can do this with the following command:
```
$ eval $(minikube docker-env)
$ docker ps
```
Docker ps will now give you a lot of images running in your minikube cluster.
If you have done it right, the list will contain images from `k8s.gcr.io`

This proxy to the docker service will ONLY be available in the current shell, so use this one for the rest of the workshop, or run the `eval` command in any new shell you open.

## 2. Running a service
First, check if there is anything running in your cluster:
```
$ k get pods
No resources found in default namespace.
```
If you wonder what kubectl is actually doing here, you can try to add the option `--v=7` to log info about what acutally happens.

### Deploy an app
We'll use `k create deployment` to create an deployment. After running the command you should try to run `k get pods -w` the `-w` option makes us "watch" the pods and log the changes happening.

```
$ k create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```
You can also check the logs from the pod by running the following commands:
```
$ k get pods
NAME                                   READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-69fbc6f4cf-4wpsf   1/1     Running   0          4m9s
$ k logs kubernetes-bootcamp-69fbc6f4cf-4wpsf
Kubernetes Bootcamp App Started At: 2020-02-01T15:43:56.831Z | Running On:  kubernetes-bootcamp-69fbc6f4cf-4wpsf
$ k get deployments
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp   1/1     1            1           8m25s
```


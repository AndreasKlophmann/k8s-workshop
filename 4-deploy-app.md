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

## 2. Prepare cluster

First, check if there is anything running in your cluster:

```
$ k get pods
No resources found in default namespace.
```

If you wonder what kubectl is actually doing here, you can try to add the option `--v=7` to log info about what acutally happens.

## 3. Deploy application

To deploy our app we will create what is known as a Deployment in kubernetes. In a Deployment (represented by a yaml manifest file)
we describe a desired state for our application, and the deployment controller will make it so. The only thing we need to
do is to instruct it to apply it for us using `kubectl apply -f deployment.yaml`

Please take a look at [4-kubernetes-bootcamp.yaml](4-kubernetes-bootcamp.yaml) for the description of our sample application.

```
$ k apply -f 4-kubernetes-bootcamp.yaml
```

The deployment controller will create a new pod for us a start the specified docker image inside it.
We can verify that we now have a deployment and a pod:

```
$ k get pods
$ k get deployments
```

You can get more details about our deployment by running:

```
$ k describe deployment kubernetes-bootcamp
```

The `describe` command can be used many places with kubectl, like:

```
$ k describe pods
$ k describe nodes
```

Read more about [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) to get a better understanding of it.

## 4. Curl the application

We can use `kubectl` to create a proxy that will forward communications into the cluster-wide, private network.
Open a second terminal window to run the proxy:

```
$ k proxy
```

This enables a connection between our host and the Kubernetes api:

```
$ curl http://localhost:8001/version
{
  "major": "1",
  "minor": "16",
  "gitVersion": "v1.16.2",
  "gitCommit": "c97fe5036ef3df2967d086711e6c0c405941e14b",
  "gitTreeState": "clean",
  "buildDate": "2019-10-15T19:09:08Z",
  "goVersion": "go1.12.10",
  "compiler": "gc",
  "platform": "linux/amd64"
}
```

The API server will automatically create a proxy for each pod, based on the pod name and service port, that is also accessible through the proxy.
We can access it locally using the following url (replacing <podname> with the actual name of the pod):

```
http://localhost:8001/api/v1/namespaces/default/pods/<podname>/proxy/
```



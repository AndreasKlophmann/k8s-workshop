# Introduction

No you know how to get information from kubernetes using kubectl(k), but much of the information can be viewed a bit easier by using the kubernetes dashboard. Let's look at it and see what we can do.

# Tasks

## 1. Different ways to view kubectl config
We can use any of the previous mentioned
```
$ k config current-context
$ k config view
```
but you can also try the same with the `--minify ` option. Try it out and see the differences


## 2. Nodes and namespaces

### Nodes
Nodes are the individual units of a Kubernetes cluster, it can be a VM or an actual computer. What makes it a node is the `kubelet`process that runs on it. 
Kubelet is responsible for communicating with the Kubernetes master and running the containers correctly. 

List all the nodes:
```
$ k get nodes
```

### Namespaces
Namespaces provide a means to separate subclusters conceptually from each other. It can for example be one namespace per application or per team. If no namespace is defined the resource is placed in the `default` namespace. 
The advantage of using namespaces are for example:
- Avoiding name clashes
- Limit resource allocation
- Manage permissions

To view something in a specific namespace you need to add the `--namespace` option when quering Kubernetes. 

You can set a default namespace for the current context:
```
$ k config set-context $(k config current-context) --namespace=my-namespace
```

## 3. Kubernetes dashboard
The built in dashboard in Kubernetes lets you click around and discover things. 
To check if the dashboard is running, use the following command:
```
$ k get pods -n kube-system
```
If there is any pods starting with `kubernetes-dashboard`the dashboard is already running, if not we need to install it:
### Install Kubernetes dashboard
To install and view the Kubernetes dashboard:
```
$ minikube dashboard
```
This should open the dashboard in your browser. 

If you want to look at the dashboard outside of minikube the command is:
```
$ k proxy
```
And then open your browser on this url:
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

If the kubernetes-dashboard is running you will see it.

### Explore the dashboard

Read about the Kubernetes dashboard [here](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) and explore it on you local minikube installation.


## Extra challenges

- Follow the guide to create deploy kubernetes dashboard locally to minikube: [k8s](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)


# Introduction

In this part of the workshop we will explore how kubectl is configured and
explore a bit around the concepts of nodes and namespaces.

# Tasks

## 1. Improve kubectl installation
Like many other [people](https://www.youtube.com/watch?v=2wgAIvXpJqU) we do not know how to pronounce 'kubectl' 
so for the rest of this workshop we'll just do the following:

Add the following to your `.zshrc / .bashrc /.bash_profile`: 
```
alias k=“kubectl”
complete -o default -F __start_kubectl k
```

There are [many](https://github.com/ahmetb/kubectl-aliases) possible aliases...    

## 2. Figure out where the kubectl config is located

- Find the kubectl config file and open it in an editor
- Explore the contents and see what you recognise in it
- Read about how to get config info [here](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

### Verify kubectl installation
You can verify the installation with the version-command above, but to be sure that everything is running correctly you should do:
```sh
$ k cluster-info
Kubernetes master is running at https://192.168.64.4:8443
KubeDNS is running at https://192.168.64.4:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```


## 3. Get info from the cluster

Use the following commands to get info and read about the different parts
- [Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)
- [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Services](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/)

```
$ k get deployments 
$ k get services 
$ k get pods 
$ k get nodes 
```

## 4. Important!
When working with Kubernetes and kubectl you should ALWAYS know which context you work in, so setting the default context to minikube is smart, and then overriding when you need to do something in another env, use the following commands:

```
$ k config get-contexts         # display list of contexts 
$ k config current-context      # display the current-context
$ k config use-context minikube # set the default context to minikube
```

Instead of switching context when doing work in qa or prod we recommend that you explicitly type in the context you want to work with, examples:

Fetch pods in current context: `$ k get pods`

Fetch pods in qa context: `$ k get pods --context qa`



## Extra challenges

- Get the status of minikube, also which addons are running or not
- Make sure minikube is running on the latest version of Kubernetes
- Find the kubectl config file and look at it
- Make sure auto completion works for kubectl
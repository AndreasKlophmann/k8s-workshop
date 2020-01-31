# Introduction

In this part of the workshop you are going to get Minikube running on your local computer. 
We are using Minikube because that is the easiest way to demonstrate how kubernetes works and what we can do with it. Minikube is a tool that runs a single-node Kubernetes cluster in a virtual machine on your personal computer.

To be able to run minikube you also need to install [Docker](https://docs.docker.com/install/) and [kubectl](https://kubernetes.io/docs/reference/kubectl/kubectl/).

## Tasks

### 1. Install Minikube
Read the instructions on how to install Minikube [here](https://kubernetes.io/docs/tasks/tools/install-minikube/), on macOS the short version is:

```
$ brew install minikube
$ brew start minikube
```

#### Verify Minikube installation

You can verify that Minikube is up and running on your computer by running the following command:

```
$ minikube status
```

### 2. Install kubectl
In order to manipulate the Kubernetes cluster you need to install the official CLI client called kubectl.

Like many other [people](https://www.youtube.com/watch?v=2wgAIvXpJqU) we do not know how to pronounce 'kubectl' so for the rest of this workshop we'll just do the following: `alias k='kubectl'`

There are [many](https://github.com/ahmetb/kubectl-aliases) possible aliases...    

Details about the installation can be found [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

```sh
$ brew install kubectl
$ k version --client
```

#### Verify kubectl installation
You can verify the installation with the version-command above, but to be sure that everything is running correctly you should do:
```sh
$ k cluster-info
Kubernetes master is running at https://192.168.64.4:8443
KubeDNS is running at https://192.168.64.4:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

### 3. Install Docker
Docker can be downloaded [here](https://docs.docker.com/install/)
Verify the installation running:

```
$ docker ps
```

### 4. Verify all up and running
Verify that all the following commands returns correctly

```
$ kubectl version
$ docker version
$ minikube version
```

### 5. Important!
When working with Kubernetes and kubectl you should ALWAYS know which context you work in, so setting the default context to minikube is smart, and then overriding when you need to do something in another env, use the following commands:

```
$ k config get-contexts         # display list of contexts 
$ k config current-context      # display the current-context
$ k config use-context minikube # set the default context to minikube
```

Instead of switching context when doing work in qa or prod we recommend that you explicitly type in the context you want to work with, examples:

Fetch pods in current context: `$ k get pods`

Fetch pods in qa context: `$ k get pods --context qa`



### Extra challenges

- Try to delete and reinstall Minikube
- Create a new cluster in Minikube and make sure it is on the latest version of Kubernetes
- Find the kubectl config file and look at it
- Make sure autocompletion works for kubectl


## Documentation to read

- [Minikube](https://kubernetes.io/docs/setup/learning-environment/minikube/)
- [Kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
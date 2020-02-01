# Introduction

In this part of the workshop you are going to get Minikube running on your local computer. 

We are using Minikube because that is the easiest way to demonstrate how kubernetes works and what we can do with it. Minikube is a tool that runs a single-node Kubernetes cluster in a virtual machine on your personal computer.

To be able to run minikube you also need to install [Docker](https://docs.docker.com/install/) and [kubectl](https://kubernetes.io/docs/reference/kubectl/kubectl/).

# Tasks

## 1. Install Minikube
Read the instructions on how to install Minikube [here](https://kubernetes.io/docs/tasks/tools/install-minikube/), on macOS the short version is:

```
$ brew install minikube
$ brew start minikube
```

### Verify Minikube installation

You can verify that Minikube is up and running on your computer by running the following command:

```
$ minikube status
```

## 2. Install kubectl
In order to manipulate the Kubernetes cluster you need to install the official CLI client called kubectl.

Details about the installation can be found [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

```sh
$ brew install kubectl
$ kubectl version --client
```

## 3. Install Docker
Docker can be downloaded [here](https://docs.docker.com/install/)
Verify the installation running:

```
$ docker ps
```

## 4. Verify all up and running
Verify that all the following commands returns correctly

```
$ kubectl version
$ docker version
$ minikube version
```

# Documentation to read

- [Minikube](https://kubernetes.io/docs/setup/learning-environment/minikube/)
- [Kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
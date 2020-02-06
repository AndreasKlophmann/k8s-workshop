# Introduction
Now we have gone through many of the basics of Kubernetes and deployment, but there is
some way to go before we know everything.
During our experience with K8s there are some things we have learned, so let's
try to go through things you should know about when using Kubernetes.

### Readiness / liveness


### Pods failing / restarting
When (I say when, not if) your pods start failing it is good to already know about what you can do,
you will of course look at the application running and its logs. 
But sometimes the pods crash and restart before sending logs to you aggregator.
So let's start to check what has happened:
```
$ k get events # Look at the events, it should say why the pod(s) restarted
$ k log <pod-name> # this is nice
$ k log --previous <pod-name> 
```
So the important here is the last line with the `--previous` option, it gives you the logs of the
pod BEFORE it restarted, there you go, look at it and see if it can help you.

Read more about it [here](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/) 

### Affinity / Nodes
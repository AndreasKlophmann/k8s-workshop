# Introduction

Now that we know how to create resource it's time to look at how we can delete them and what the rules are when doing so.
We can delete individual resources using `kubectl delete` and this is fine for testing and debugging. But in a
production setting we will usually just change the yaml file describing our service, apply it again and let kubernetes
handle the resource management. 

# Tasks

## 1. Deleting resources

First open up a separate terminal window watch for changes in pods:

```
$ k get pods -w
```

Then try to delete a single pod using the following command:

```
$ k delete pod <podname>
```

**What happened? How many pods did we have and how many did we end up with?**

We deleted the pod, but kubernetes immediately created another one for us. To understand why we need to look at the
output from the following command:

```
$ k describe pods | grep Controlled
Controlled By:  ReplicaSet/kubernetes-bootcamp-6d64485df4
```

### The ReplicaSet

So there is another resource controlling how many pods our deployment should have - namely a [**ReplicaSet**](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
We can query it for info:

```
$ k describe replicasets
Name:           kubernetes-bootcamp-6d64485df4
Namespace:      default
Selector:       app=kubernetes-bootcamp,pod-template-hash=6d64485df4
Labels:         app=kubernetes-bootcamp
                pod-template-hash=6d64485df4
Annotations:    deployment.kubernetes.io/desired-replicas: 1
                deployment.kubernetes.io/max-replicas: 2
                deployment.kubernetes.io/revision: 2
Controlled By:  Deployment/kubernetes-bootcamp
Replicas:       1 current / 1 desired
Pods Status:    1 Running / 0 Waiting / 0 Succeeded / 0 Failed
...
```

We can delete the ReplicaSet using the following command:

```
$ k delete replicaset <replicaset-name>
```

As before the net effect of the command was nothing. We deleted the ReplicaSet and the Pod, but kubernetes immediately
recreated them for us. So something must be controlling it:

```
$ k describe replicasets | grep Controlled
Controlled By:  Deployment/kubernetes-bootcamp
```

### Deployment again

So we have discovered that a pod is controlled by a RecplicaSet Which in turn is controlled by a Deployment.
Unless we have very specific and advanced needs, deployment is the recommended level of abstraction that we should target.
If we want fever replicas of an app we can simply scale down the deployment (more on scaling in part 9).
If we want to remove all pods of a certain app we can delete the deployment:

```
$ k delete deployment <deployment-name>
```

Verify that all related resources are gone using:

```
$ k get replicasets
$ k get pods
```

## 2. Challenges

* Redeploy the app using the descriptor `4-kubernetes-bootcamp.yaml`
* Change the replica count to 0 and re-apply the deployment. What happens?
* Change the replica count to 10. What happens?

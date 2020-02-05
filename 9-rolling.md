# Introduction

Now we have a deployment managing a set of pods. Let's see what we can do to it to meet
for example load conditions. 
Lets first try a simple scaling

## Scaling
Before you start with this, open a new terminal and watch the pods as we have done earlier

```
$ k get pods -w -o wide # in the new terminal
$ k scale deploy kubernetes-bootcamp --replicas=4 
```

Another way of scaling is changing the deploy yaml, or by opening an editor,
try to scale both ways:
```
$ k edit deployment kubernetes-bootcamp # edit number of replicas in the editor
$ k apply -f 4-deploy-app.yaml # edit the file first
```

## Events
We have earlier talked about logging from the pods using the `k logs <podname>` command, but
there are more things happening inside the cluster, a lot of it are represented as events,
check the events with:
```
$ k get events -w # Try to run it in a new terminal while scaling
```


## Roll out a new version
Open and apply the new yaml-file `9-update-app.yaml` that deploys version 2 of the app.
```
$ k get pods -w -o wide # in a new terminal 
$ k get events -w # in a new terminal
$ k apply -f 9-update-app.yaml
```

* What happens here? We're trying to update the app, but we are getting some kind
of weird message?

Try to check the status of the rollout:
```
$ k rollout status deployments/k8s-workshop
Waiting for deployment "k8s-workshop" rollout to finish: 2 out of 4 new replicas have been updated...
```
The `ImagePullBackOff` and the `ErrImagePull` error says we are trying to fetch an 
image that is not there. 

## Undo the roll out
Let's see if we can cancel the rollout:

```
$ k rollout undo deployments/k8s-workshop
deployment.apps/k8s-workshop rolled back
```

## Roll out the correct version
Now change the yaml-file to try and fetch this
image instead: `jocatalin/kubernetes-bootcamp:v2`

And then try again:
(Remember to keep watching both pods and events)
```
$ k apply -f 9-update-app.yaml
```
* What happened now? Did it work? 

check the status again:
```
$ k rollout status deployments/k8s-workshop
deployment "k8s-workshop" successfully rolled out
```

### More info
You can read more about rolling updates [here](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/) 
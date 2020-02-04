# Introduction

Now that we have a running application we want to be able to check its status. In this section we'll learn some commands
that can be handy for querying and debugging our app inside k8s.

# Tasks

## 1. Details about the pod

As we learned in the previous section we can list the pods with:

```
$ k get pods
$ k get pods -o wide # show more information
```

And details about a single pod with:

```
$ k describe pod <podname>
```

## 2. Application logs

Seeing details about the kubernetes metadata can be helpful, but to see how our application is doing we probably want
to check its logs. We can easily do that with `kubectl logs`:

```
$ k logs <podname>
$ k logs -f <podname> # to follow the logs as new lines are written
```

This will display any logging that the app sends to `stdout` inside the container.
`kubectl logs` takes a number of parameters to limit the amount of log retrieved:

```
$ k logs --tail=50 <podname> # only show the last 50 lines
$ k logs --since=1h <podname> # only show loglines from the last hour
```

## 3. Executing remote commands

You can run commands inside a running container using `kubectl exec`. We can use this to execute one-off commands or
even open an interactive terminal:

```
$ k exec <podname> -- uname -a
Linux kubernetes-bootcamp-69fbc6f4cf-92sqx 4.19.81 #1 SMP Tue Dec 10 16:09:50 PST 2019 x86_64 GNU/Linux
```

Opening a shell requires the `-i` and `-t` option to attach stdin and treat it as a tty:

```
$ k exec -it <podname> sh
root@<podname>:/#
```

## 4. Running one-off containers for debugging

If we want to get inside the kubernetes cluster but don't want to attach directly to our app we can start any docker
image in a separate pod ad automatically delete it after we're finished:

```
$ k run -it --rm --restart=Never testing --image=cfmanteiga/alpine-bash-curl-jq bash
```

## 5. Port forwarding to pods

We can forward any local port to a port on a pod using `kubectl port-forward`. This can be handy when debugging an application:

```
$ k port-forward <podname> 9999:8080 # will bind local port 9999 to pod port 8080
```

## 6. Challenges

1. Figure out which linux distro our app is running on (hint: /etc/os-release)
2. What is the ip-address of our app inside kubernetes?
3. invoke the service running on port 8080 on our app (hint: there are at least three ways described above and in section 4)

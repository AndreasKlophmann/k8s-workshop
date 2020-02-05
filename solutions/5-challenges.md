### Figure out which linux distro our app is running on (hint: /etc/os-release)

```
$ k exec -it kubernetes-bootcamp-6d64485df4-fk4t8 bash
$ cat /etc/os-release
PRETTY_NAME="Debian GNU/Linux 8 (jessie)"
NAME="Debian GNU/Linux"
VERSION_ID="8"
VERSION="8 (jessie)"
ID=debian
HOME_URL="http://www.debian.org/"
SUPPORT_URL="http://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

### What is the ip-address of our app inside kubernetes?
```
$ k get pods -w -o wide
NAME                                   READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
kubernetes-bootcamp-6d64485df4-fk4t8   1/1     Running   0          2m40s   172.17.0.7   minikube   <none>           <none>
```

### invoke the service running on port 8080 on our app (hint: there are at least three ways described above and in section 4)
```
$ k proxy # in a separate terminal
$ http://localhost:8001/api/v1/namespaces/default/pods/<podname>/proxy/
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-6d64485df4-fk4t8 | v=1
```
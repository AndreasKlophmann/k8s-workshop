### Redeploy the app using the descriptor `4-deploy-app.yaml`
```
$ k delete deployment k8s-workshop # if you havn't done so already
$ k apply -f 4-deploy-app.yaml
```

### Change the replica count to x
- Change the number of replicas on line 6 in the 4-deploy-app.yaml and
```
$ k apply -f 4-deploy-app.yaml
```


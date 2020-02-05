### Redeploy the app using the descriptor `4-kubernetes-bootcamp.yaml`
```
$ k delete deploy kubernetes-bootcamp
$ k apply -f 4-kubernetes-bootcamp.yaml
```

### Change the replica count to x
- Change the count in the 4-kubernetes-bootcamp.yaml and
```
$ k apply -f 4-kubernetes-bootcamp.yaml
```


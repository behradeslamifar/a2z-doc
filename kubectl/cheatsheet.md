```
kubectl -n <namespace> get pods \
  -o=custom-columns=NAME:.metadata.name,STATUS:.status.phase,NODE:.spec.nodeName
```

گرفتن پیکربندی ها
```
kubectl -n <namespace> get deployments <servicename> -o yaml
kubectl -n <namespace> get service <servicename> -o yaml
kubectl -n <namespace> get configmap <configname> -o yaml 
```

راه اندازی مجدد یک pod 
```
kubectl -n <namespace> delete pod <pod name>              # get new image from registrey
kubectl -n <namespace> apply -f /path/to/deploy.yml       # apply change in yaml file
```

تغییر تعداد replica
```
kubectl -n <namespace> --current-replicas=1 --replicas=3 -f /path/to/deploy.yml
```

```
vi /path/to/deploy.yml                                    # change replicas directive
...
spec:
  minReadySeconds: 5
  replicas: 4
...

kubectl -n <namespace> apply -f /path/to/deploy.yml       # apply change in yaml file
```

اجرای یک pod به وسیله Replication Controller
```
kubectl run kube1 --image=name --port=9000 --generator=run/v1
```
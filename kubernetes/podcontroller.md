#### Pod
در این حالت در صورت delete کردن pod، مجددا راه اندازی نمی شود
```
apiVersion: v1
kind: Pod
metadata:
  name: kubia-manual
spec:
  containers:
  - image: luksa/kubia
    name: kubia
    ports:
    - containerPort: 8080
      protocol: TCP
```



##### =ReplicationController

```
apiVersion: v1
kind: ReplicationController
metadata:
  name: kubia
spec:
  replicas: 3
  selector:
    app: kubia
template:
  metadata:
    labels: 
      app: kubia
  spec:
    containers: 
    - name: kubia
      image: luksa/kubia
      ports:
      - containerPort: 8080
```


#### ReplicaSet


```
apiVersion: apps/v1beta2
kind: ReplicaSet
metadata:
  name: kubia
spec:
  replicas: 3
  selector:
    matchLabels:
      app: kubia
  template:
    metadata:
     labels:
       app: kubia
    spec:
      containers:
      - name: kubia
        image: luksa/kubia
```

#### Statefullset
```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-statefulset
  namespace: cafe
spec:
  serviceName: "nginx"
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: htmldoc-pvc
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: htmldoc-pvc
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: ext-storageclass
      resources:
        requests:
          storage: 1Gi
```

#### Daemonset

##### =Job

```
apiVersion: batch/v1
kind: Job
metadata:
  name: batch-job
spec:
  template:
    metadata:
      labels:
        app: batch-job
  spec:
    restartPolicy: OnFailure
    containers:
    - name: main
      image: luksa/batch-job
```


#### CronJob
```
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: batch-job-every-fifteen-minutes
spec:
  schedule: "0,15,30,45 * * * *"
  jobTemplate:
    spec:
      template:
        metadata:
          labels:
            app: periodic-batch-job
        spec:
          restartPolicy: OnFailure
          containers:
          - name: main
            image: luksa/batch-job
```


#### Deployment
#### Headless
```
apiVersion: v1
kind: Service
metadata:
  name: externalip
  namespace: kube-system
spec:
  clusterIP: None
  ports:
  - name: https
    port: 443
    protocol: TCP
---
kind: Endpoints
apiVersion: v1
metadata:
  name: external-ip
  namespace: kube-system
subsets:
  - addresses:
      - ip: 172.16.10.20
    ports:
      - port: 443
        name: https
        protocol: TCP

```
  
#### externalName
موفق به استفاده نشدم
```
kind: Service
apiVersion: v1
metadata:
  name: externalname
  namespace: default
spec:
  type: ExternalName
  externalName: 192.168.0.1.xip.io
```
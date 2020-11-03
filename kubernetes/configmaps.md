ساخت یک configmap: مستقیم با دستور kubectl
```
kubectl create configmap myconfigmap --from-literal=foo=bar --from-literal=bar=baz --from-literal=one=two
```
  
```
kubectl create configmap my-config --from-file=config-file.conf
```
  
```
kubectl create configmap my-config --from-file=customkey=config-file.conf
```
  

استفاده ترکیبی از حالتهای بالا
```
kubectl create configmap my-config
 --from-file=foo.json
 --from-file=bar=foobar.conf
 --from-file=config-opts/
 --from-literal=some=thing
```
  
استفاده در Pod
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx 
    env:
    - name: INTERVAL
    valueFrom:
      configMapKeyRef:
        name: nginx-config
        key: sleep-interval
```
  
ارسال همه کلیدهای یک config-map به عنوان متغیر محیطی
```
spec:
  containers:
  - image: some-image
    envFrom:
    - prefix: CONFIG_
      configMapRef:
      name: my-config-map
```
  
ارسال config-map به عنوان آرگومان‌های خط فرمان
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    env:
    - name: INTERVAL
      valueFrom:
        configMapKeyRef:
        name: nginx-config
        key: sleep-interval
    args: ["$(INTERVAL)"]
```
  
ساخت configmap: با ساخت فایل Yaml
```
apiVersion: v1
data:
  my-nginx.conf: |
    server {
      listen 80;
      server_name www.linuxmotto.ir;
      
      gzip on;
      gzip_types text/plain application/xml;

      location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
      }
     }
  sleep-interval: |
    25
kind: ConfigMap
metadata:
  name: nginx-config
```
  
استفاده از configmap در volume
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - image: nginx:alpine
      name: web-server
      volumeMounts:

      - name: config
        mountPath: /etc/nginx/conf.d
        subPath: gzip.conf
        readOnly: true

  volumes:
  - name: config
    configMap:
      name: nginx-config
      defaultMode: "660"
      items:
        key: my-nginx.conf
        path: gzip.conf
```
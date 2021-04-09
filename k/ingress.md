### Certification Based Authentication with Ingress-Nginx
  * from this [source](https://medium.com/@awkwardferny/configuring-certificate-based-mutual-authentication-with-kubernetes-ingress-nginx-20e7e38fdfca)
##### Create Certificate
```
$ openssl req -x509 -sha256 -newkey rsa:4096 -keyout ca.key -out ca.crt -days 356 -nodes -subj '/CN=Linuxmotto Cert Authority'

$ openssl req -new -newkey rsa:4096 -keyout server.key -out server.csr -nodes -subj '/CN=linuxmotto.com'
$ openssl x509 -req -sha256 -days 365 -in server.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out server.crt

# Generate the Client Key, and Certificate and Sign with the CA Certificate
$ openssl req -new -newkey rsa:4096 -keyout client.key -out client.csr -nodes -subj '/CN=Fern'
$ openssl x509 -req -sha256 -days 365 -in client.csr -CA ca.crt -CAkey ca.key -set_serial 02 -out client.crt
```

##### Create Secret
```
kubectl create secret generic my-certs --from-file=tls.crt=server.crt --from-file=tls.key=server.key --from-file=ca.crt=ca.crt
```

##### Create Ingress
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/auth-tls-verify-client: "on"
    nginx.ingress.kubernetes.io/auth-tls-secret: "default/my-certs"
  name: meow-ingress
  namespace: default
spec:
  rules:
  - host: linuxmotto.ir
    http:
      paths:
      - backend:
          serviceName: linuxmotto-svc
          servicePort: 80
        path: /
  tls:
  - hosts:
    - meow.com
    secretName: my-certs
```
برای استفاده از etcdctl باید متغیر های زیر را تعریف کنید
```
ETCDCTL_API=3
ETCDCTL_CA=/path/to/ca.pem
ETCDCTL_CERT=/path/to/cert.pem
ETCDCTL_KEY=/path/to/key.pem
```
   
بررسی وضعیت
```
$ etcdctl --version
etcdctl version: 3.2.17
API version: 2

etcdctl member list
etcdctl -w table member list
etcdctl cluster-health
```
  
  
```
$ etcdctl version
etcdctl version: 3.2.17
API version: 3.2

etcdctl member list
etcdctl -w table member list
etcdctl endpoint health
etcdctl endpoint status
```
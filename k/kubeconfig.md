#### Kubeconfig
در صورتی که basic auth در apiserver با گزینه --basic-auth-file=SOMEFILE فعال شده باشد.

```
apiVersion: v1
clusters:
- cluster:
    insecure-skip-tls-verify: true
    server: https://master:6443
  name: ubuntu
contexts:
- context:
    cluster: ubuntu
    user: ubuntu
  name: ubuntu
current-context: ubuntu
kind: Config
preferences: {}
users:
- name: ubuntu
  user:
    password: password
    username: admin
```


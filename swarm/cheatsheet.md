### راه اندازی مجدد یک namespace 
```
docker stack rm <namespace>
docker stack deploy -c </path/to/deployfile.yml> <namespace>
```

راه اندازی مجدد یک container
```
docker service rm <container_name>
docker config rm <configmap_name>
docker stack deploy -c <deploy_file.yaml> --with-registry-auth <namespace_name>
```

به روز کردن یا restart کردن یک ماشین
```
docker service update <container_name>
```
# Docker Registry

## Docker Registry Rate-limit promblem
1. Define Google registry as a mirror.(problem: not all images exist on this mirror)
```
# vi /etc/docker/daemon.json
{
  "registry-mirrors": ["https://mirro.gcr.io"]
}
```

2. Run Docker Registry as a Cache
follow [Docker Registry as a Cache](#docker-registry-as-a-cache) instruction.

## Docker Registry as a Cache
Required directory
```
mkdir -p registry/{data,certs,conf}
cd registry
```

Create registry config file (conf/config.yml)
```
version: 0.1
log:
  fields:
    service: registry
storage:
  delete:
    enabled: true
  cache:
    blobdescriptor: inmemory
  filesystem:
    rootdirectory: /var/lib/registry
http:
  addr: :5000
  headers:
    X-Content-Type-Options: [nosniff]
    Access-Control-Allow-Origin: ['http://127.0.0.1:8001']
    Access-Control-Allow-Methods: ['HEAD', 'GET', 'OPTIONS', 'DELETE']
    Access-Control-Allow-Headers: ['Authorization', 'Accept']
    Access-Control-Max-Age: [1728000]
    Access-Control-Allow-Credentials: [true]
    Access-Control-Expose-Headers: ['Docker-Content-Digest']
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
proxy:
  remoteurl: https://registry-1.docker.io
```

Create docker-compose.yaml
```
version: '3'
services:
  registry:
    restart: always
    image: registry:2
    ports:
      - 5000:5000
    environment:
      REGISTRY_HTTP_TLS_CERTIFICATE: /certs/domain.crt
      REGISTRY_HTTP_TLS_KEY: /certs/domain.key
    volumes:
      - ./data:/var/lib/registry
      - ./certs:/certs
      - ./auth:/auth
      - ./conf:/etc/docker/registry
    networks:
      - registry-ui-net
  ui:
    image: joxit/docker-registry-ui:static
    ports:
      - 8080:80
    environment:
      - REGISTRY_TITLE=My Private Docker Registry
      - REGISTRY_URL=https://registry:5000
      - DELETE_IMAGES=true
    depends_on:
      - registry
    networks:
      - registry-ui-net

networks:
  registry-ui-net:
```

Create certificate
```
# openssl req -x509 -new -days 365 -nodes -out certs/domain.crt -keyout certs/domain.key -subj '/CN=registry.example.com/' -addext "subjectAltName = IP:192.168.43.202"
```

Edit client docker config (/etc/docker/daemon.json) and restart docker (systemctl restart docker)
```
{
  "insecure-registries": [ "192.168.43.202:5000" ],
  "registry-mirrors": ["https://192.168.43.202:5000"]
}
```

Pull image for test and test registry data directory
```
client ~:# docker pull alpine
```

```
registry ~:# # ls data/docker/registry/v2/repositories/library
alpine
```

## Private Docker Registry with Authentication
Run private docker registry with basic auth and web interfaces
1. Create required directory
```
mkdir regsitry/{certs,data,auth,conf}
cd registry
```

2. Create config file (conf/config.yml)
```
version: 0.1
log:
  fields:
    service: registry
storage:
  delete:
    enabled: true
  cache:
    blobdescriptor: inmemory
  filesystem:
    rootdirectory: /var/lib/registry
http:
  addr: :5000
  headers:
    X-Content-Type-Options: [nosniff]
    Access-Control-Allow-Origin: ['http://127.0.0.1:8001']
    Access-Control-Allow-Methods: ['HEAD', 'GET', 'OPTIONS', 'DELETE']
    Access-Control-Allow-Headers: ['Authorization', 'Accept']
    Access-Control-Max-Age: [1728000]
    Access-Control-Allow-Credentials: [true]
    Access-Control-Expose-Headers: ['Docker-Content-Digest']
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3

```

3. Create certificate
```
# openssl req -x509 -new -days 365 -nodes -out certs/domain.crt -keyout certs/domain.key -subj '/CN=registry.example.com/' -addext "subjectAltName = IP:192.168.43.202"
Generating a RSA private key
.......+++++
...........+++++
writing new private key to 'certs/domain.key'
-----
```

4. Create docker-compose.yaml. file
```
version: '3'
services:
  registry:
    restart: always
    image: registry:2
    ports:
      - 5000:5000
    environment:
      REGISTRY_HTTP_TLS_CERTIFICATE: /certs/domain.crt
      REGISTRY_HTTP_TLS_KEY: /certs/domain.key
      REGISTRY_AUTH: htpasswd
      REGISTRY_AUTH_HTPASSWD_PATH: /auth/htpasswd
      REGISTRY_AUTH_HTPASSWD_REALM: Registry Realm
    volumes:
      - ./data:/var/lib/registry
      - ./certs:/certs
      - ./auth:/auth
      - ./conf:/etc/docker/registry
    networks:
      - registry-ui-net
  ui:
    image: joxit/docker-registry-ui:static
    ports:
      - 80:80
    environment:
      - REGISTRY_TITLE=My Private Docker Registry
      - REGISTRY_URL=https://registry:5000
      - DELETE_IMAGES=true
    depends_on:
      - registry
    networks:
      - registry-ui-net

networks:
  registry-ui-net:
```

5. Create authentication file
```
# htpasswd -Bc auth/htpasswd behrad
New password:
Re-type new password:
Adding password for user behrad
```

6. Start docker machines
```
docker-compose up -d
```


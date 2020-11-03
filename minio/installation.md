### ##### نصب Single Node به کمک docker-compose
راحت ترین راه برای نصب استفاده از Docker است. 

```
version: '3.4'

services:
 minio:
  image: minio/minio
  restart: always
  volumes:
   - /opt/minio/data:/export
   - /opt/minio/certs:/root/.minio/certs
  ports:
   - "9000:9000"
  environment:
   MINIO_ACCESS_KEY: ${ACCESS_KEY}
   MINIO_SECRET_KEY: ${SECRET_KEY}
  command: server /export
```

و در نهایت اجرا
```
docker-compose up -d
```
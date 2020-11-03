گرفتن certificate به صورت standalone
```
certbot certonly --standalone -d demo.tld.domain --non-interactive \ 
  --agree-tos --email user@email.com     --http-01-port=<port> --http-01-addr=<ipaddress>
```

برای ایجاد فایل مناسب haproxy
```
cat /etc/letsencrypt/live/demo.tld.domain/fullchain.pem \
  /etc/letsencrypt/live/demo.tld.domain/privkey.pem \
  | tee /etc/ssl/demo.tld.domain/demo.tld.domain.pem
```

```
certbot renew --tls-sni-01-port=8888
```

```
#!/usr/bin/env bash

# Renew the certificate
certbot renew --force-renewal --tls-sni-01-port=8888

# Concatenate new cert files, with less output (avoiding the use tee and its output to stdout)
bash -c "cat /etc/letsencrypt/live/demo.scalinglaravel.com/fullchain.pem /etc/letsencrypt/live/demo.scalinglaravel.com/privkey.pem > /etc/ssl/demo.scalinglaravel.com/demo.scalinglaravel.com.pem"

# Reload  HAProxy
service haproxy reload
```

```
0 0 1 * * root bash /opt/update-certs.sh
```

#### منابع
  * [Letsencrypt with haproxy](https://serversforhackers.com/c/letsencrypt-with-haproxy)
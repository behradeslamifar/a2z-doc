محدود کردن حجم log های container

```
version: '3'

services:
  mongodb:
    image: mongo:3.4
    restart: always
    volumes:
      - /opt/mongo/data:/data/db
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    mem_limit: 3g
    memswap_limit: 3g
    cpu_count: 4
    logging:
      options:
        max-size: 1g

```

ارسال log ها به graylog 

```
version: '3.1'

services:

  mysql:
    image: mariadb:10.2.14
    restart: always
    network_mode: "host"
    environment:
      MYSQL_ROOT_PASSWORD: ${ROOT_PASS}
    volumes:
      - /opt/mysql/data:/var/lib/mysql
    logging:
      driver: gelf
      options:
        gelf-address: "udp://192.168.100.10:12201"
        tag: "mysql"
```

تعریف docker volume
```
version: '3.1'

services:
   elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.2.4
    ports:
      - 9200:9200
    environment:
      - "ES_JAVA_OPTS=-Xms4g -Xmx4g -Duser.timezone=Asia/Tehran"
    volumes:
      - elasticsearch_data:/usr/share/elasticsearch/data
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone:ro

volumes:
  elasticsearch_data: {}
```

برای اینکه به این volume زمانی که داکر down است استفاده کنید، می تونید به روش زیرعمل کنید
```
docker volume ls
docker run --rm -it -v elasticsearch_elasticsearch_data:/var/elastic alpine sh
```
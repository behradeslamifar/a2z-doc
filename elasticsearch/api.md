### ##### مانیتور کردن سرویس
#### دستورات مانیتورینگ
```
user@hostname:~$ curl http://<ip_address>:9200/_cluster/health?pretty
{
  "cluster_name": "elasticsearch",
  "status": "yellow",
  "timed_out": false,
  "number_of_nodes": 1,
  "number_of_data_nodes": 1,
  "active_primary_shards": 29,
  "active_shards": 29,
  "relocating_shards": 0,
  "initializing_shards": 0,
  "unassigned_shards": 1,
  "delayed_unassigned_shards": 0,
  "number_of_pending_tasks": 0,
  "number_of_in_flight_fetch": 0,
  "task_max_waiting_in_queue_millis": 0,
  "active_shards_percent_as_number": 96.66666666666667
}
```

### ##### کار با index ها
```
http://<ip_address>:9200/_cat/indices
```

#### پاک کردن index
```
curl -XDELETE http://<ip_address>:9200/<index_name>
```

#### تغییر تعداد Replica
```
curl -XPUT 'http://<ip_address>:9200/<index_name>/_settings' -H 'Content-Type: application/json' \
  -d '{ "index" : { "number_of_replicas" : 2 } }'
```

#### گرفتن وضعیت shard ها
```
$ curl -XGET localhost:9200/_cat/shards?h=index,shard,prirep,state,unassigned.reason
infra-2019.12.30                     4 r STARTED    
infra-2019.12.30                     0 p STARTED    
infra-2019.12.30                     0 r STARTED    
.monitoring-kibana-6-2020.01.05            0 r STARTED    
.monitoring-kibana-6-2020.01.05            0 p STARTED    
user-2019.12.26            3 p STARTED    
user-2019.12.26            3 r STARTED    
user-2019.12.26            1 p STARTED
user-2019.12.26            1 r UNASSIGNED NODE_LEFT
```

### حذف یک نود بدون از دست دادن دیتا
```
curl -XPUT localhost:9200/_cluster/settings -H 'Content-Type: application/json' -d '{
  "transient" :{
      "cluster.routing.allocation.exclude._ip" : "10.0.0.1"
   }
}'
```

### ==کار با template=
```
$ curl http://127.0.0.1:9200/_cat/templates
.monitoring-es          [.monitoring-es-6-*]       0          6000051
security_audit_log      [.security_audit_log*]     2147483647 
logstash-index-template [.logstash]                0          
logstash                [logstash-*]               0          60001
.monitoring-logstash    [.monitoring-logstash-6-*] 0          6000051
.monitoring-alerts      [.monitoring-alerts-6]     0          6000051
security-index-template [.security-*]              1000       
.monitoring-kibana      [.monitoring-kibana-6-*]   0          6000051
```

```
$ curl http:/127.0.0.1:9200/_template/logstash?pretty
{
  "logstash" : {
    "order" : 0,
    "version" : 60001,
    "index_patterns" : [
      "logstash-*"
    ],
    "settings" : {
      "index" : {
        "refresh_interval" : "5s"
      }
    },
    "mappings" : {
...
```

```
$ curl  -H "Content-Type: application/json" -XPUT http://127.0.0.1:9200/_template/all  -d '{
  "template": "*",
  "order": 0,
  "settings": {
    "number_of_shards": 5,
    "number_of_replicas": 2
  }
}'
```
کانفیگ کلاستر زمانی که با داکر پیاده شده 
```
#xpack.license.self_generated.type: basic
#xpack.monitoring.enabled: true
#xpack.security.enabled: false
#xpack.monitoring.exporters.my_local:
#  type: local
#  use_ingest: false

cluster.name: my-cluster
node.master: true
node.data: true
node.ingest: false
search.remote.connect: false
node.name: node1

path.data: /usr/share/elasticsearch/data
path.logs: /var/log/elasticsearch
bootstrap.memory_lock: true

network.host: 0.0.0.0
network.publish_host: 192.168.10.1
http.port: 9200

discovery.zen.minimum_master_nodes: 1
discovery.zen.join_timeout: 20s
discovery.zen.ping.unicast.hosts: ["192.168.10.1","192.168.10.2","192.168.10.3"]
discovery.zen.master_election.ignore_non_master_pings: false
discovery.zen.fd.ping_interval: 1s
discovery.zen.fd.ping_timeout: 30s
discovery.zen.fd.ping_retries: 3
```
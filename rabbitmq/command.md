دستوراتی که بر روی single node مفید هستند
```
rabbitmq-diagnostics environment
rabbitmq-diagnostics status
```

دستوراتی که برروی cluster state مفید هستند
```
rabbitmqctl list_connections 
rabbitmqctl list_mqtt_connections
rabbitmqctl list_stomp_connections
rabbitmqctl list_users
rabbitmqctl list_vhosts
```

دستورات مربوط به cluster
```
rabbitmqctl cluster_status
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl join_cluster rabbit@rabbit1
rabbitmqctl start_app
```
تنظیمات Kernel Parameter
```
# cat /etc/sysctl.d/redis.conf
vm.overcommit_memory = 1
net.core.somaxconn = 1024
```

غیر فعال کردن Transparent Hugepage
```
echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo "echo never > /sys/kernel/mm/transparent_hugepage/enabled" >> /etc/rc.d/rc.local
```

حداقل پیکربندی
```
bind 0.0.0.0
port 6379

logfile /var/log/redis/redis.log
pidfile /var/run/redis/redis-server.pid
unixsocket /var/run/redis/redis.sock
unixsocketperm 700

dir /var/lib/redis
dbfilename dump.rdb

maxclients 100
maxmemory-policy volatile-lru
loglevel notice
```
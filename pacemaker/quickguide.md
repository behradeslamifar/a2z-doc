#### نصب اولیه یک کلاستر
```
root@node1:~# apt install pacemaker corosync pcs fence-agents
root@node1:~# adduser hacluster

root@node1:~# pcs cluster auth node1 node2
root@node1:~# pcs cluster setup --name linuxmotto node1 node2

root@node1:~# crm_verify -L -V
   error: unpack_resources: Resource start-up disabled since no STONITH resources have been defined
   error: unpack_resources: Either configure some or disable STONITH with the stonith-enabled option
   error: unpack_resources: NOTE: Clusters with shared data need STONITH to ensure data integrity
Errors found during check: config not valid
root@node1:~# pcs property set stonith-enabled=false
```

#### مانیتور کردن وضعیت موجود Pacemaker و corosync
```
root@node1:~# pcs status
root@node1:~# pcs cluster status

root@node1:~# corosync-cfgtool -s
root@node1:~# corosync-cmapctl | grep members
```

#### اضافه کردن یک resource
```
root@node1:~# pcs resource create ClusterIP ocf:heartbeat:IPaddr2 \
    ip=192.168.13.51 cidr_netmask=32 op monitor interval=30s
root@node1:~# pcs resource defaults resource-stickiness=100
```

```
root@node1:~# pcs resource create WebSite ocf:heartbeat:apache  \
      configfile=/etc/httpd/conf/httpd.conf \
      statusurl="http://localhost/server-status" \
      op monitor interval=1min
root@node1:~# pcs resource op defaults timeout=240s
```
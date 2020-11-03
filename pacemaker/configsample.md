### MariadDB Replication
ساخت کاربر
```
MariaDB [(none)]> grant SUPER, REPLICATION SLAVE, REPLICATION CLIENT, PROCESS on *.* to 'repl_user'@'%' identified by '123';
MariaDB [(none)]> grant replication client, replication slave, SUPER, PROCESS, RELOAD on *.* to 'repl_user'@'localhost' identified by '123';
```

اضافه کردن Mysql RA به Pacemaker
```
pcs node attribute node1 p_mysql_mysql_master_IP="192.168.56.20"
pcs node attribute node2 p_mysql_mysql_master_IP="192.168.56.21"

pcs resource create ClusterIP ocf:heartbeat:IPaddr2 ip=192.168.56.23 cidr_netmask=32 op monitor interval=30s

pcs resource create p_mysql ocf:percona:mysql \
            config="/etc/mysql/my.cnf" pid="/var/run/mysql/mysqld.pid" socket="/var/run/mysqld/mysqld.sock" replication_user="repl_user" \
            replication_passwd="123" max_slave_lag="60" evict_outdated_slaves="false" binary="/usr/sbin/mysqld" \
            test_user="test_user" test_passwd="123"
pcs resource op add p_mysql start interval="0" timeout="60s"
pcs resource op add p_mysql stop interval="0" timeout="60s"

pcs resource update p_mysql --master meta master-max="1" master-node-max="1" clone-max="2" clone-node-max="1" notify="true" globally-unique="false" target-role="Master" is-managed="true"

pcs resource master ms_MySQL p_mysql
pcs constraint colocation add  master  ms_MySQL with ClusterIP
pcs resource op add  p_mysql monitor interval="5s" role="Master" OCF_CHECK_LEVEL="1"
pcs resource op add  p_mysql monitor interval="2s" role="Slave" OCF_CHECK_LEVEL="1"
```
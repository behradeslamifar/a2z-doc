# Master/Slave Replication with Mariabackup
`Testing environment`: Ubuntu 20.04  
`Mariadb version`: 10.3  
`Repository`: Ubuntu official repository  

### install mariadb on master and slave
```
apt-get install -y mariadb-server maridb-backup
```

### Master
#### Enable binary log 
Eidt `/etc/mysql/mariadb.conf.d/50-server.conf`  

```
bind = 0.0.0.0
...

server-id              = 1
log_bin                = /var/log/mysql/mysql-bin.log
expire_logs_days       = 10
max_binlog_size        = 100M
binlog_format          = mixed
```

Restart mariadb
```
systemctl restart mariadb
```

#### Create Replication User
```
CREATE USER 'repl'@'dbserver2' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.*  TO 'repl'@'dbserver2';
```

#### Take Backup
```
mkdir -p /var/mariadb/backup
mariabackup --backup    --target-dir=/var/mariadb/backup/    --user=root
rsync -adv /var/mariadb/backup user@slave:
```

#### copy gtid number. In this example is "0-1-4,1-1-3"
```
cat /var/mariadb/backup/xtrabackup_binlog_info 
mysql-bin.000001	358	0-1-4,1-1-3
```

### Slave
#### edit config
Edit `/etc/mysql/mariadb.conf.d/50-server.conf`  
```
server-id              = 2
log_bin                = /var/log/mysql/mysql-bin.log
binlog_format          = mixed
expire_logs_days       = 10
max_binlog_size        = 100M

relay-log              = /var/log/mysql/mysql-relay-bin.log
read-only              = 1
```

#### Stop service and delete datadir
systemctl stop mariadb
rm -r /var/lib/mysql/*

#### recover backup and start service
```
mariabackup --copy-back    --target-dir=/home/vagrant/backup/
systemctl restart mariadb
```

#### Run these command
Run these command on mysql cli  
```
SET GLOBAL gtid_slave_pos = "0-1-4,1-1-3";
CHANGE MASTER TO 
   MASTER_HOST="192.168.10.221", 
   MASTER_PORT=3306, 
   MASTER_USER="repl",  
   MASTER_PASSWORD="password", 
   MASTER_USE_GTID=slave_pos;
START SLAVE;
```

# Openconnect Client

## Run Openconnect client with systemd service
Create necessary directory
```
mkdir /run/openconnect /etc/openconnect
```

Create service unit file
```
# vi /etc/systemd/system/oc@.service
[Unit]
Description=OpenConnect client to Cisco AnyConnect VPN
Documentation=man:openconnect(8)
After=network.target

[Service]
EnvironmentFile=-/etc/openconnect/%i.conf
Type=forking
PIDFile=/run/openconnect/%i.pid
ExecStartPre=/bin/sh -c 'test -d /run/openconnect/ || mkdir /run/openconnect; /touch /run/openconnect/%i.pid'
ExecStart=/bin/sh -c 'echo $PASSWORD | /usr/sbin/openconnect $SERVER --pid-file /run/openconnect/%i.pid --user $USERNAME $OPTIONS'
ExecReload=/bin/kill -USR2 $MAINPID
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Create config file
```
# vi /etc/openconnect/server.conf
USERNAME=<username>
PASSWORD=<password>
SERVER=<server_url>
OPTIONS="--background --passwd-on-stdin <other_options>"
```

Enable service
```
systemctl enable --now oc@server.service
```


تنظیمات فایروال
```
# iptables -I OUTPUT -d <ipaddr> -p Ah -j ACCEPT
# iptables -I OUTPUT -d <ipaddr> -p vrrp -j ACCEPT
```

تنظیمات یکی از نود ها
```
global_defs {
    notification_email {
        notif@linuxmotto.ir
    }
    notification_email_from behrad@linuxmotto.ir
    smtp_server 127.0.0.1
    smtp_connect_timeout 30
    #lvs_id string
    enable_script_security
}

vrrp_script chk_haproxy {
    script "/usr/bin/pgrep haproxy"
    # script "</dev/tcp/127.0.0.1/443"
    interval 2
    rise 3
}

vrrp_sync_group VG1 {
    group {
        VI_1
        VI_2
    }
}

vrrp_instance VI_1 {
    interface enp0s3
    state MASTER
    priority 200

    virtual_router_id 33
    unicast_src_ip 192.168.56.20
    unicast_peer {
        192.168.56.21
    }

    authentication {
        auth_type PASS
        auth_pass password
    }

    # The virtual ip address shared between the two loadbalancers
    virtual_ipaddress {
        192.168.57.22
    }

    track_script {
        chk_haproxy
    }
}

vrrp_instance VI_2 {
    interface enp0s8
    state BACKUP
    priority 200

    virtual_router_id 34
    unicast_src_ip 192.168.57.21
    unicast_peer {
        192.168.57.20
    }

    authentication {
        auth_type PASS
        auth_pass password
    }

    # The virtual ip address shared between the two loadbalancers
    virtual_ipaddress {
        192.168.57.22
    }

    track_script {
        chk_haproxy
    }
}
```
```
# vi /etc/sysctl.d/20-linuxmotto.conf
vm.swappiness = 1
vm.dirty_ratio = 10
vm.dirty_background_ratio = 5

# allow testing with buffers up to 64MB
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864

# increase Linux autotuning TCP buffer limit to 32MB
net.ipv4.tcp_rmem = 4096 87380 33554432
net.ipv4.tcp_wmem = 4096 65536 33554432

# recommended default congestion control is htcp
net.ipv4.tcp_congestion_control=htcp

# recommended for hosts with jumbo frames enabled
net.ipv4.tcp_mtu_probing=1

# recommended for CentOS7/Debian8 hosts
net.core.default_qdisc = fq

net.ipv4.route.flush=1
net.ipv4.ip_forward = 1

# try to reuse time-wait connections, but don't recycle them (recycle can break clients behind NAT)
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_tw_reuse=1
      
# Decrease the time default value for tcp_fin_timeout connection
net.ipv4.tcp_fin_timeout = 7

# Decrease the time default value for connections to keep alive
net.ipv4.tcp_keepalive_time = 300
net.ipv4.tcp_keepalive_probes = 5
net.ipv4.tcp_keepalive_intvl = 15

# elastic recommand
vm.max_map_count=262144

#
vm.overcommit_memory = 1

# socket connection
net.core.somaxconn=8192

# syn attack
net.ipv4.tcp_max_syn_backlog = 2048
net.ipv4.tcp_synack_retries = 3
net.netfilter.nf_conntrack_tcp_timeout_syn_recv=45
net.ipv4.conf.all.rp_filter=1
net.ipv4.conf.default.rp_filter=1
net.ipv4.conf.all.rp_filter=1
net.ipv4.tcp_syncookies=1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.accept_source_route = 0
fs.protected_hardlinks=1
fs.protected_symlinks=1
```
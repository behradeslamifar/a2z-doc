# Network Manager
## Install Network-Manager
```
$ sudo apt-get install network-manager
```

## Use Network Manager on Console
Show current connections
```
nmcli connection show
```

Add wireless connection from cli
```
nmcli d wifi connect <ssid> password <password>
```

Change connection configuration
```
nmcli connection modify <connection_name> ipv4.method manual ipv4.addresses 192.168.13.230/24 ipv4.dns 192.168.13.1 ipv4.gateway 192.168.13.1
```

Show device configuration
```
nmclie device show
```

## Netplan and Network-Manager
```
network:
  version: 2
  renderer: NetworkManager
```

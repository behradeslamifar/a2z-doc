# Network Legacy

## /etc/network/interfaces
```
# The loopback network interface
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet manual
iface eth0 inet static
	address 192.168.10.80
	netmask 255.255.255.0
	gateway 192.168.10.1
	dns-nameservers 8.8.8.8	
	up ifconfig eth0 192.168.10.150/24 up
	post-up route add -net 192.168.100.0/24 gw 192.168.10.1

auto tunnel
iface tunnel inet ppp
	provider myprovider
	post-up route add -net 172.16.10.0/24 dev ppp0
	post-up route add -host 172.16.11.32 dev ppp0
	post-up route add -net 10.0.0.0/8 dev ppp0
	post-up route add -net 172.27.0.0/16 dev ppp0

auto wimax
iface wimax inet dhcp
	wpa-ssid Myssid
	wpa-psk 7b5f159e6634633d328edce16fc6110284712d8097b

auto mobile
iface mobile inet dhcp
	wpa-ssid Myssid
	wpa-psk StrongPassword

auto open
iface open inet dhcp
	wireless-essid Open Wifi
	wireless-mode open
	# wireless-mode Ad-Hoc
	#wpa-ssid "Open Wifi"

iface wlan0 inet manual
```

Create crypted wpa password
```
echo "password" | wpa_passphrase <essid>
```



# Setup OpenVPN with Certificate Authentication
This document about, Setting up your own Certificate Authority (CA) and generating certificates and keys for an OpenVPN server and multiple clients

## Version Table
| Tools     |   Version    |
| --------- |:------------:|
| Ubuntu    | 18.04 Xenial |
| OpenVPN   | 2.4.4-2ubuntu1.6 |

## Required Packages
Install openvpn and easy-rsa
```
apt-get install -y openvpn easy-rsa
```

## Create Certificate Authority (CA) and certificates for clients

#### Configure easy-rsa 
Make a copy of easy-rsa tools and config
```
cp -r /usr/share/easy-rsa /etc/openvpn/
cd /etc/openvpn/easy-rsa
ln -s openssl-1.0.0.cnf openssl.cnf
```

Edit vars file. Don't leave any of these fields blank.
```
# vi vars
...
export KEY_COUNTRY="IR"
export KEY_PROVINCE="P"
export KEY_CITY="C"
export KEY_ORG="O"
export KEY_EMAIL="myemail@example.com"
export KEY_OU="OU"

export KEY_NAME="EasyRSA"
export KEY_ALTNAMES="EasyRSA"
export KEY_CN=certificateAuthority
```

#### Initialize variables and build CA. CA files places on keys folder.
```
source ./vars
./clean-all
./build-ca
```

#### Generate key and certificate for openvpn service
```
./build-key-server server
```

Generate Diffie Helman file
```
./build-dh
```

#### Generate key and certificate for client
```
./build-key behrad
```

## Configure OpenVPN service
Create openvpn server config in /etc/openvpn/server.conf
```
mode server
multihome
ca easy-rsa/keys/ca.crt
cert easy-rsa/keys/server.crt
key easy-rsa/keys/server.key
dh easy-rsa/keys/dh2048.pem
client-config-dir ccd
keepalive 10 60
user nobody
group nogroup
tls-server
comp-lzo
status openvpn-status.log
log /var/log/openvpn.log
verb 1
dev tun0
max-clients 2048
ccd-exclusive
persist-key
persist-tun
mute 20
ifconfig-pool-persist /etc/openvpn/address-pool-assignments.txt
push "register-dns"
proto udp
port 1194
cipher AES-128-CBC
client-to-client
server 172.16.113.0 255.255.255.0
management 127.0.0.1 1195
```

#### Create Address Pool File for clients
Edit /etc/openvpn/address-pool-assignments.txt. Add client per line.
```
behrad,172.16.113.4
narges,172.16.113.5
```

#### Enable and Start OpenVPN
```
systemctl enable --now openvpn@server
```

## Create ovpn file for clients
This is sample behrad.ovpn file. Please replace **server_ip_address** , ca certificate, client certificate and key.
```
client
resolv-retry 20
keepalive 10 60
nobind
mute-replay-warnings
ns-cert-type server
comp-lzo
max-routes 500
verb 1
persist-key
persist-tun
explicit-exit-notify 1
dev tun
proto udp
port 1194
cipher AES-128-CBC
remote <server_ip_address> 1194

<ca>
*copy ca certificate pem format here (ca.crt content)*
</ca>

<cert>
*copy client certificate pem format here (behrad.crt content)*
</cert>

<key>
*copy client key pem format here (behrad.key content)*
</key>
```

## Resources
* [OpenVPN Community Resources](https://openvpn.net/community-resources/how-to/#setting-up-your-own-certificate-authority-ca-and-generating-certificates-and-keys-for-an-openvpn-server-and-multiple-clients)

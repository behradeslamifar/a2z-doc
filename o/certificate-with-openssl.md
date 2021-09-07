# Create Certificate with OpenSSL

## Create Certificate Authority (CA)
Create Selfsigned Certificate authority
```
openssl req -x509 -new -nodes -days 3650 -keyout rootCAKey.pem -out rootCACert.pem -subj '/CN=certificateAuthority/C=CO/ST=ST/L=L/O=O/OU=OU'
```

## Create DH file
openssl dhparam -out dhparams.pem 2048

## Create Certificate with Alternative Names
Create config file server.conf
```
[ req ]
default_bits = 2048
prompt = no
default_md = sha256
req_extensions = req_ext
distinguished_name = dn

[ dn ]
O = group
CN = hostname

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1=hostname
DNS.2=hostname.default
DNS.3=example.com
IP.1=10.1.1.1
IP.2=10.2.2.2

[ v3_ext ]
authorityKeyIdentifier=keyid,issuer:always
basicConstraints=CA:FALSE
keyUsage=keyEncipherment,dataEncipherment
extendedKeyUsage=serverAuth,clientAuth
subjectAltName=@alt_names
```

Create key and certificate
```
openssl genrsa -out server.key 2048
openssl req -new -key server.key -out server.csr -config server.conf
openssl x509 -req -in server.csr -CA rootCACert.pem -CAkey rootCAKey.pem -CAcreateserial -out server.crt -days 365 -extensions v3_ext -extfile server.conf
```

## Create Client Certificate 
Create config file user.conf
```
[ req ]
default_bits = 2048
prompt = no
default_md = sha256
distinguished_name = dn

[ dn ]
O = group
CN = behrad

[ v3_ext ]
keyUsage=critical,keyEncipherment,digitalSignature
extendedKeyUsage=clientAuth
```

Create key and certificate
```
openssl req -nodes -new -keyout behrad.key -out behrad.csr -config .conf
openssl x509 -req -in behrad.csr -CA rootCACert.pem -CAkey rootCAKey.pem -CAcreateserial -out behrad.crt -days 365 -extensions v3_ext -extfile user.conf
```

## Verify Certificate
```
openssl x509 -noout -text -in server.crt
```

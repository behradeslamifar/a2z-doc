```
mydomain = linuxmotto.ir
myhostname = laptop.$mydomain
mydestination = localhost $mydomain $myhostname
inet_interfaces = all
inet_protocols = ipv4
mynetworks = 127.0.0.1
mailbox_size_limit = 0

smtpd_tls_security_level = may
smtpd_use_tls=yes
smtpd_tls_auth_only = yes
smtpd_tls_cert_file=/etc/pki/dovecot/certs/dovecot.pem
smtpd_tls_key_file=/etc/pki/dovecot/private/dovecot.pem
smtpd_tls_loglevel = 0
smtpd_tls_received_header = yes

smtp_tls_security_level = may
smtp_use_tls=yes
#smtp_tls_auth_only = yes
smtp_tls_cert_file=/etc/pki/dovecot/certs/dovecot.pem
smtp_tls_key_file=/etc/pki/dovecot/private/dovecot.pem
smtp_tls_loglevel = 0


smtpd_sasl_auth_enable = yes
smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
# old outlook express
broken_sasl_auth_clients = yes
smtpd_sasl_security_options = noanonymous

home_mailbox = Mail/
smtpd_recipient_restrictions = permit_sasl_authenticated, permit_mynetworks, reject_unauth_destination
```
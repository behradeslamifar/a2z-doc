```
# main.cf
#myorigin = /etc/mailname
smtpd_banner = $myhostname ESMTP $mail_name (Ubuntu)
biff = no
append_dot_mydomain = no
#delay_warning_time = 4h
readme_directory = no

default_process_limit = 150
qmgr_message_active_limit = 20000
qmgr_message_recipient_limit = 20000
disable_dns_lookups = no
anvil_status_update_time = 600s
smtpd_client_connection_rate_limit = 30
anvil_rate_time_unit = 60s
smtpd_client_connection_count_limit = 30

smtpd_tls_security_level = may
smtpd_use_tls=yes
smtpd_tls_auth_only = yes
smtpd_tls_cert_file=/etc/dovecot/dovecot.pem
smtpd_tls_key_file=/etc/dovecot/private/dovecot.pem
smtpd_tls_loglevel = 0
smtpd_tls_received_header = yes

smtp_tls_security_level = may
smtp_use_tls=yes
#smtp_tls_auth_only = yes
smtp_tls_cert_file=/etc/dovecot/dovecot.pem
smtp_tls_key_file=/etc/dovecot/private/dovecot.pem
smtp_tls_loglevel = 0
#smtp_tls_received_header = yes
# See /usr/share/doc/postfix/TLS_README.gz in the postfix-doc package for
# information on enabling SSL in the smtp client.

myhostname = mail.linuxmotto.ir
#smtp_helo_name = linuxmotto.com
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
mydestination = localhost.localdomain, localhost
relayhost = 
mynetworks =
mailbox_size_limit = 0
recipient_delimiter = +
inet_interfaces = all
#default_transport = error
#relay_transport = error
inet_protocols = all

smtpd_sasl_auth_enable = yes
smtpd_sasl_path = private/auth
smtpd_sasl_type = dovecot
broken_sasl_auth_clients = yes
smtpd_sasl_security_options = noanonymous
#smtpd_tls_auth_only = yes

virtual_alias_maps = mysql:/etc/postfix/mysql_virtual_alias_maps.cf
virtual_minimum_uid = 135
virtual_uid_maps = static:135
virtual_gid_maps = static:135
virtual_mailbox_base = /home/vmail
virtual_mailbox_domains = mysql:/etc/postfix/mysql_virtual_domains_maps.cf
virtual_mailbox_maps = mysql:/etc/postfix/mysql_virtual_mailbox_maps.cf
virtual_transport = lmtp:unix:private/dovecot-lmtp
#virtual_transport = virtual

smtpd_relay_restrictions = permit_mynetworks permit_sasl_authenticated defer_unauth_destination
smtpd_recipient_restrictions = permit_mynetworks,
        permit_sasl_authenticated,
        reject_unauth_destination,
        reject_unauth_pipelining,
        reject_non_fqdn_recipient,
	check_policy_service inet:127.0.0.1:12340
smtpd_sender_restrictions = permit_mynetworks,
        permit_sasl_authenticated,
        reject_unknown_helo_hostname,
        reject_unknown_sender_domain
#        reject_sender_login_mismatch,
#        reject_unknown_recipient_domain,
smtpd_client_restrictions = reject_rbl_client cbl.abuseat.org,
        reject_rbl_client pbl.spamhaus.org,
        reject_rbl_client sbl.spamhaus.org,
        permit_mynetworks,
        permit_sasl_authenticated
        reject_unknown_client_hostname


# DKIM and spamassassin,milter
milter_default_action = accept
# ( Postfix ≥ 2.6 milter_protocol = 6, Postfix ≤ 2.5 milter_protocol = 2 )
milter_protocol = 6
smtpd_milters = inet:localhost:12345, unix:/spamass/spamass.sock
non_smtpd_milters = inet:localhost:12345

# Attachment size 
message_size_limit = 60000000
```

```
# mysql_mailbox_virtual_maps.cf
user = postfixadmin
password = <password>
hosts = localhost
dbname = postfixadmin
table = mailbox
select_field = maildir
where_field = username
```

```
# mysql_virtual_alias_maps.cf
#user = postfixadmin
#password = <password>
#hosts = localhost
#dbname = postfixadmin
#table = alias
#select_field = goto
#where_field = address

# alternative
user = postfixadmin
password = <password>
hosts = unix:/var/run/mysqld/mysqld.sock
#hosts = localhost
dbname = postfixadmin
query = SELECT goto FROM alias WHERE address = '%s'
```

```
user = postfixadmin
password = <password>
hosts = localhost
dbname = postfixadmin
table = domain
select_field = domain
where_field = domain
additional_conditions = and backupmx = '0' and active = '1'
```

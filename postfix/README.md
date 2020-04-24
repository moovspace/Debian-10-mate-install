### Postfix TLS
nano /etc/postfix/main.cf
```bash
# SMTP from your server to others
smtp_tls_key_file = /etc/postfix/myserver.key
smtp_tls_cert_file = /etc/postfix/myserver-full.pem
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
smtp_tls_security_level = encrypt
smtp_tls_note_starttls_offer = yes
smtp_tls_auth_only = no
# smtp_tls_mandatory_protocols=!SSLv2,!SSLv3
# smtp_tls_protocols=!SSLv2,!SSLv3
smtp_tls_loglevel = 1
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
smtp_tls_session_cache_timeout = 3600s

# SMTP from other servers to yours
smtpd_tls_key_file = /etc/postfix/myserver.key
smtpd_tls_cert_file = /etc/postfix/myserver-full.pem
smtpd_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
smtpd_tls_security_level = may
smtpd_tls_note_starttls_offer = yes
smtpd_tls_auth_only = yes
# smtpd_tls_mandatory_protocols=!SSLv2,!SSLv3
# smtpd_tls_protocols=!SSLv2,!SSLv3
smtpd_tls_loglevel = 1
smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache
smtpd_tls_session_cache_timeout = 3600s

# Relay ban
smtpd_relay_restrictions = permit_mynetworks, permit_sasl_authenticated, reject_unauth_destination, defer_unauth_destination

# Recipient
smtpd_recipient_restrictions = permit_mynetworks, reject_unknown_helo_hostname, reject_unauth_destination, defer_unauth_destination

# Block clients that speak too early.
smtpd_data_restrictions = reject_unauth_pipelining

myhostname = vege.xx
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
myorigin = /etc/mailname

# Local smtp emails domains
# mydestination = $myhostname, vege.xx, domain.xx, localhost

# Send only smtp server
mydestination = localhost

# Disable relay
relayhost =

# Allow from
mynetworks = 127.0.0.0/8 [::1]/128

# inet_interfaces = all
# inet_protocols = all

# Only ip4 (default: all)
inet_protocols = ipv4
# Recive from (default: all)
inet_interfaces = 127.0.0.1, [::1]


mailbox_size_limit = 0
recipient_delimiter = +
```

### Links
- https://zurgl.com/how-to-configure-tls-encryption-in-postfix/
- https://www.gigaone.pl/wsparcie-techniczne/instalacja-certyfikatu-ssl/postfix

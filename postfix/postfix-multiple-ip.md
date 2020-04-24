# Postfix multiple ips

## Set main.cf
sudo nano /etc/postfix/main.cf
```
sender_dependent_default_transport_maps = randmap:{relay1,relay2,relay3,relay4,relay5}
smtp_connection_cache_on_demand=no
```

## Set master.cf
sudo nano /etc/postfix/master.cf
```
relay1     unix  -       -       n       -       -       smtp
  -o smtp_bind_address=IP1
  -o smtp_helo_name=foo1.woo.xx
  -o syslog_name=relay1
relay2     unix  -       -       n       -       -       smtp
  -o smtp_bind_address=IP2
  -o smtp_helo_name=foo2.woo.xx
  -o syslog_name=relay2
relay3     unix  -       -       n       -       -       smtp
  -o smtp_bind_address=IP3
  -o smtp_helo_name=foo3.woo.xx
  -o syslog_name=relay3
relay4     unix  -       -       n       -       -       smtp
  -o smtp_bind_address=IP4
  -o smtp_helo_name=foo4.woo.xx
  -o syslog_name=relay4
relay5     unix  -       -       n       -       -       smtp
  -o smtp_bind_address=IP5
  -o smtp_helo_name=foo5.woo.xx
  -o syslog_name=relay5
```
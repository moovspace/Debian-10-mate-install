# Install smtp server
```
# Install smtp
sudo apt install postfix

# Install mail client
sudo apt install mailutils

# Test postfix
sudo postconf mail_version

# Test config
sudo postconf -d
```

# Postfix virtual users catchall emails

## Create file
sudo nano /etc/postfix/virtual
```
# Add aliases
@woo.xx linux-username
@domain.xx linux-username
```

# Run config
```
postmap /etc/postfix/virtual
```

## Add or change
sudo nano /etc/postfix/main.cf
```
# Trusted networks (default: 127.0.0.0/8 192.168.0.0/24 [::1]/128 [fe80::]/64)
mynetworks = 127.0.0.0/8 192.168.0.0/24 [::1]/128

# Only ip4 (default: all)
inet_protocols = ipv4

# Recive from (default: all)
inet_interfaces = 127.0.0.1, [::1]

# Set domain
mydestination = woo.xx, domain.xx, $myhostname, localhost

# Set aliases
virtual_alias_maps = hash:/etc/postfix/virtual

# Disable relay
smtpd_recipient_restrictions = permit_mynetworks, permit_sasl_authenticated, reject_unauth_destination
smtpd_sender_restrictions = reject_unknown_sender_domain
```

## Restart
```
sudo service postfix restart
```

## Send email
```
echo "Reminder: Leaving at 1 PM today" | sudo mail -s "Early departure" m@woo.xx
```

## Show user emails
```
cat /var/mail/linux-username
```

## Firewall
```
# Install
sudo apt install ufw
sudo ufw enable

# Disable incoming emails
sudo ufw deny 25
sudo ufw deny 587

# Delete if needed
sudo ufw delete deny 25
sudo ufw delete deny 587
```
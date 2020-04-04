### Instalacja iptables
```bash
sudo apt install iptables

# Autoload iptables on restart
sudo apt install iptables-persistent

# Save
Debian/Ubuntu: iptables-save > /etc/iptables/rules.v4
RHEL/CentOS: iptables-save > /etc/sysconfig/iptables

Debian/Ubuntu: ip6tables-save > /etc/iptables/rules.v6
RHEL/CentOS: ip6tables-save > /etc/sysconfig/ip6tables

# Restore
Debian/Ubuntu: iptables-restore < /etc/iptables/rules.v4
RHEL/CentOS: iptables-restore < /etc/sysconfig/iptables

Debian/Ubuntu: ip6tables-restore < /etc/iptables/rules.v6
RHEL/CentOS: ip6tables-restore < /etc/sysconfig/ip6tables
```

### Import z bash
```bash
# Uprawnienia
sudo chmod +x ip4.sh
sudo chmod +x ip6.sh

# Import
sudo bash ./ip4.sh
sudo bash ./ip6.sh
```

### Secure kernel
nano /etc/sysctl.conf
```bash
# Secure ip4
net.ipv4.ip_forward = 0
net.ipv4.icmp_echo_ignore_all = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.all.secure_redirects = 1
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 2048
net.ipv4.tcp_synack_retries = 2
net.ipv4.conf.all.rp_filter = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.conf.all.log_martians = 1
net.ipv4.tcp_rfc1337 = 1
# Router only
# net.ipv4.eth0.proxy_arp = 1
# net.ipv4.eth1.proxy_arp = 1

# Secure ip6
net.ipv6.ip_forward = 0
net.ipv6.icmp_echo_ignore_all = 1
net.ipv6.icmp_echo_ignore_broadcasts = 1
net.ipv6.conf.all.accept_source_route = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.all.send_redirects = 0
net.ipv6.conf.all.secure_redirects = 1
net.ipv6.tcp_syncookies = 1
net.ipv6.tcp_max_syn_backlog = 2048
net.ipv6.tcp_synack_retries = 2
net.ipv6.conf.all.rp_filter = 1
net.ipv6.icmp_ignore_bogus_error_responses = 1
net.ipv6.conf.all.log_martians = 1
net.ipv6.tcp_rfc1337 = 1
# Router only
# net.ipv4.eth0.proxy_arp = 1
# net.ipv4.eth1.proxy_arp = 1
```

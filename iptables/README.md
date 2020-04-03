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

# Debian 10 Mate desktop install
How to install Debian 10 mate desktop with Wifi, LEMP web server and firewall.

## Create bootable USB from distro iso (as root user)
```diff
# Login as root
su

# See partitions, disks
fdisk -l
df -h

# Burn iso on usb
dd  if=debian.iso of=/dev/sdb

# Or (if install errors)
dd  if=debian.iso of=/dev/sdb1
```

### Install debian 10
```diff
! Advanced Options -> Graphical Expert install -> in -> Software selection select:
+ desktop environment, Mate desktop, standard system utilities
```

### Grub doouble boot (windows, debian)(as root user)
nano /etc/default/grub
```bash
# Change number 1, 2 ...
GRUB_DEFAULT=2

# next 
sudo update-grub
```

### Sudo user (as root user)
```bash
su

# in file
nano /etc/sudoers

# Add user to sudo
user ALL=(ALL:ALL) ALL

# Logout root
exit
```

### Apt https
```bash
# Install tools
sudo apt install apt-transport-https net-tools dnsutils git curl openssl mate-tweak

# Remove package
sudo apt remove avahi-daemon
sudo apt purge avahi-daemon

# Clean
sudo apt autoremove

# Check active services
sudo netstat -tulpn
```

### Sublime text 3
```bash
# Key and sources list
sudo wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

# update sources and install
sudo apt update
sudo apt install sublime-text
```

### Wifi
```bash
# Install
sudo apt install firmware-b43-installer
sudo apt install firmware-b43legacy-installer
sudo apt install firmware-brcm80211
sudo apt install firmware-iwlwifi
sudo modprobe -r iwlwifi
sudo modprobe iwlwifi

# Wifi interface
sudo ifconfig

# Search your wifi SSID
sudo iwlist wlp2s0b1 scan
```

### Network Manager
```diff
! Add new connection WiFi in ***NetworkManager*** 
+ Mate desktop right top corner and set SSID and credentials
```

### Lemp (nginx, php, mariadb)
```bash
# Install
sudo apt install nginx php-fpm php-mysql php-gd php-json php-curl php-mbstring mariadb-server

# Restart service
sudo systemctl restart nginx
sudo systemctl restart php7.3-fpm

# Add mysql root user
sudo mysql

# Create or update user in mysql>
GRANT ALL ON *.* TO 'root'@'localhost' IDENTIFIED BY 'toor' WITH GRANT OPTION;
GRANT ALL ON *.* TO 'root'@'127.0.0.1' IDENTIFIED BY 'toor' WITH GRANT OPTION;
FLUSH PRIVILEGES;

# Exit from mysql>
exit

# Secure mysql server
sudo mysql_secure_installation

# Host folder
mkdir -p /var/www/html/domain.xx

# Permission
sudo chown -R $USER:$USER /var/www/html
sudo chmod -R 775 /var/www/html
```

### Nginx domain virtualhost
sudo nano /etc/nginx/sites-available/default
```bash
server {
    listen 80;
    listen [::]:80;
    
    # Document dir
    root /var/www/html/domain.xx;
    
    # Run first
    index index.php index.html index.htm;
    
    # Domain, host
    server_name domain.xx www.domain.xx;

    location = /favicon.ico {
        # root /favicon;
        rewrite . /favicon/favicon.ico;
    }

    location / {
        # Get file or folder or error
        # try_files $uri $uri/ =404;
        
        # Get file or folder or redirect uri to url param in index.php
        try_files $uri $uri/ /index.php?url=$uri&$args;
    }

    location ~ \.php$ {
        # Php-fpm
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;

        # Php-fpm sockets
        # fastcgi_param HTTP_PROXY "";
        # fastcgi_pass 127.0.0.1:9000;
        # fastcgi_index index.php;        
        # include fastcgi_params;
    }
}
```

### SSL/TLS virtualhost (all virtualhosts in one file)
sudo nano /etc/nginx/sites-available/default
```bash
server {
    # Http
    listen  80;
    listen  [::]:80;
    
    # Https
    listen  443 ssl http2;
    listen  [::]:443 ssl http2;
    
    # Document root
    root /var/www/html/domain.xx;
    server_name         domain.xx www.domain.xx;
    http2_push_preload on;
    
    # With self-signed
    ssl_certificate     /etc/ssl/certs/ssl-cert-snakeoil.pem;
    ssl_certificate_key /etc/ssl/private/ssl-cert-snakeoil.key;
    
    # With sslforfree.com or letsencrypt.org ssl
    # ssl_certificate     www.domain.xx.crt; # cert or with bundle cert
    # ssl_certificate_key www.domain.xx.key;
    
    ...
}
```

### Nginx cache
```bash
proxy_cache_path /path/to/cache levels=1:2 keys_zone=my_cache:10m max_size=10g inactive=60m use_temp_path=off;

server {
    # ...
    location / {
        proxy_cache my_cache;		
        proxy_cache_revalidate on;
        proxy_cache_min_uses 3;
        proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
        proxy_cache_background_update on;
        proxy_cache_lock on;

		# Cache requests types
		proxy_cache_methods GET HEAD POST;		
		# No cache with params
		proxy_cache_bypass $cookie_nocache $arg_nocache;
		# Add cache status
		add_header X-Cache-Status $upstream_cache_status;

		# Stream
        proxy_pass http://my_upstream;
    }

	location /images/ {
		proxy_cache my_cache;
		# Ignore client Cache-Controll:
		proxy_ignore_headers Cache-Control;
		proxy_cache_valid any 30m;
		# ...
	}
}
```

### Test Ssl cert
```bash
openssl s_client -connect domain.xx:443
```

### For more nginx ssl
http://nginx.org/en/docs/http/configuring_https_servers.html

### Domain hosts for local domains
sudo nano /etc/hosts
```bash
127.0.0.1 localhost domain.xx domain1.xx domain2.xx
```

### Nginx Ssl, load balancer samples
https://github.com/moovspace/HowTo/blob/master/debian_lemp/ssl-virtual-host.sample

### Enable nginx virtualhost
```bash
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/

# Test nginx config
sudo nginx -t

# Restart nginx server and php-fpm
sudo systemctl restart nginx
sudo systemctl restart php7.3-fpm
```

### Mysql user
sudo mysql -u root -p
```sql
# Create database
CREATE DATABASE example_database CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
SHOW DATABASES;

# Create or update user
GRANT ALL ON *.* TO 'root'@'localhost' IDENTIFIED BY 'toor' WITH GRANT OPTION;
GRANT ALL ON *.* TO 'root'@'127.0.0.1' IDENTIFIED BY 'toor' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

### For more
https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mariadb-php-lemp-stack-on-debian-10


### Firewall desktop
```bash
sudo apt install ufw

sudo nano /etc/default/ufw
IPV6=YES

# Add rules
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow ssh if needed
sudo ufw allow 22

# Allow from net mask
sudo ufw allow from 2.1.0.0/16 to any port 22

# Allow http, https
sudo ufw allow 80
sudo ufw allow 443

# Deny from
sudo ufw deny from 2.1.0.0

# Delete
sudo ufw delete allow 80

# Enable
sudo ufw enable

# Show rules
sudo ufw status numbered
sudo ufw status verbose

# Show IP addresses
sudo ip addr
```

### For more
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-debian-10


### Bash prompt colored
cd; nano .bashrc
```bash
# Add on the end of file
export PS1="\[\e[32m\][\[\e[m\]\[\e[31m\]\u\[\e[m\]\[\e[33m\]@\[\e[m\]\[\e[32m\]\h\[\e[m\]:\[\e[36m\]\w\[\e[m\]\[\e[32m\]]\[\e[m\]\[\e[32;47m\]\\$\[\e[m\] "
```

### Themes
cd; mkdir .themes
```bash
sudo apt install mate-desktop-environment-extras
sudo apt install arc-theme
sudo apt install adapta-gtk-theme
sudo apt install numix-gtk-theme
```

### Icons
```bash
cd; mkdir .icons

# Awesome
https://www.gnome-look.org/s/Gnome/p/1333376
https://www.gnome-look.org/s/Gnome/p/1300159
https://www.gnome-look.org/s/Gnome/p/1327720

# install
sudo apt install gnome-icon-theme
sudo apt install moka-icon-theme
sudo apt install numix-icon-theme

# or download
https://www.gnome-look.org/p/1284047
https://www.gnome-look.org/p/1305251
```

### Fonts
```bash
wget http://mirrors.edge.kernel.org/ubuntu/pool/main/u/ubuntu-font-family-sources/ttf-ubuntu-font-family_0.83-0ubuntu2_all.deb
sudo dpkg -i ttf-ubuntu-font-family*.deb
```

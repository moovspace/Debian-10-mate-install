### Apache2 włącz moduły
```bash
sudo apt install mor_rewrite mod_ssl mod_headers mod_expires
sudo a2enmod rewrite ssl expires http2 headers
```

### Apache2 import ustawień (apache2.conf lub httpd.conf)
nano /etc/apache2/apache2.conf
```bash
Include conf.d/*.conf
```

### Katalog domeny i uprawnienia (as root: su -)
```bash
mkdir /var/www/html/domain.xx
chown -R www-data:username /var/www/html/domain.xx
chmod -R 775 /var/www/html/domain.xx
```

### Virtualhost
nano /etc/apache2/sites-available/domain.xx.conf
```
<VirtualHost *:80>
    ServerName domain.xx
    ServerAlias www.domain.xx
    Redirect permanent / https://domain.xx/
    
    # RewriteEngine On
    # RewriteCond %{HTTPS} !=on
    # RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L] 
</VirtualHost> 

<VirtualHost *:443>
    ServerName domain.xx
    ServerAlias www.domain.xx
    DocumentRoot /var/www/html/domain.xx

    ServerAdmin webmaster@domain.xx
    ErrorLog /var/log/domain.xx.log
    CustomLog /var/log/domain.xx.custom.log combined

    SSLEngine on
    SSLCertificateKeyFile /path/to/private.key
    SSLCertificateFile /path/to/cert.pem
    SSLProtocol all -SSLv3 -TLSv1 -TLSv1.1

    # Include cache, headers, expires
    # Include conf.d/*.conf
</VirtualHost>
```

### Enable virtualhost
```bash
a2ensite domain.xx
a2ensite domain.xx.conf
```

### Disable virtualhost
```bash
a2dissite domain.xx
a2dissite domain.xx.conf
```

### Apache2 configtest
```bash
apache2ctl configtest
```

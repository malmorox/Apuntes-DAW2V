# DokuWiki

<https://www.dokuwiki.org/dokuwiki>

## Installation Guide

**Requirements** => PHP 7.2 or higher and Apache2
PHP extensions required (ubuntu-server):

- php-json
- php-bz2
- php-zip
- php-mbstring
- php-gd
- php-intl
- php-sqlite3
- php-xml

**Instalación:**

sudo adduser wikicosas

sudo mkdir /var/www/wikicosas/

cd /var/www/

sudo chmod g+w wikicosas

sudo chown -R www-data:wikicosas wikicosas/

cd /etc/apache2/sites-available/

sudo cp 000-default.conf 00X-wikicosas.conf

sudo vim 00X-wikicosas.conf

```apache
<VirtualHost *:80>
    ServerName wikicosas.es
    ServerAdmin wiki@mail.org
    DocumentRoot /var/www/wikicosas
    ErrorLog ${APACHE_LOG_DIR}/wikicosas.error.log
    CustomLog ${APACHE_LOG_DIR}/wikicosas.access.log combined

    <Directory /var/www/wikicosas>
        Options .Indexes
        AllowOverride All
        AccessFileName .htaccess.dist
    </Directory>
</VirtualHost>
```

sudo a2ensite 00X-wikicosas.conf

sudo systemctl reload apache2

**Subir archivos con Filezilla**

cd /var/www/wikicosas

sudo chown www-data -R .

*Entrar al dominio y ver que se puedan crear cuentas y editar*

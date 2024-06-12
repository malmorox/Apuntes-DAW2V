# WORDPRESS

## [Instalación de Wordpress](https://es.wordpress.org/download/) <span style="font-size: small;"> (En el cliente) </span>

### ¿Qué necesitamos?

Necesitaremos PHP 7.4 o superior y  MySQL 5.7-MariaDB 10.4 o superior.

**mysql_secure_installation:**

```bash
Switch to unix_socket authentication [Y/n] => Y
Change the root password? [Y/n] => n
Remove anonymous users? [Y/n] => Y
Disallow root login remotely? [Y/n] => Y
Remove test database and access to it? [Y/n] => Y
Reload privilege tables now? [Y/n] => Y
```

Si da algun error puede ser necesario instalar libapache2-mod-php:

```apache
sudo apt install libapache2-mod-php 
sudo apt install php-mysql
```

### Configuracion de usuario

```apache
$=> sudo adduser chistes
        New password: chistes
        Retype new password: chistes
        Is the information correct? [Y/n] Y
```

```apache
sudo mkdir /var/www/chistes
```
```apache 
chmod 755 /var/www/chistes
```
```apache
sudo chown -R www-data:www-data /var/www/chistes
```

Le tenemos que meter en el grupo al usuario que demos los permisos de la carpeta (si no usamos el www-data):
```apache
sudo usermod -a -G www-data chistes
```
Así sería: 
```apache
sudo chown -R chistes:www-data /var/www/chistes
```

### Configuración de sitio con apache2

```apache
sudo nano /etc/apache2/sites-available/005-chistes.conf
```

```apache
<VirtualHost *:80>
    ServerName chist.es
    DocumentRoot /var/www/chistes
    
    AssignUserId chistes chistes

    ErrorLog ${APACHE_LOG_DIR}/chistes.error.log
    CustomLog ${APACHE_LOG_DIR}/chistes.access.log combined
</VirtualHost>
```

```apache
$=> sudo a2ensite 005-chistes.conf
```

```apache
$=> sudo systemctl reload apache2
```

**Instalación y configuración de WordPress**

*Conectar con FileZilla y subir los archivos al servidor*

**Crear DB**

```SQL
CREATE DATABASE wordpress;

CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'pass';

GRANT ALL PRIVILEGES on wordpress.* TO 'wordpress'@'localhost';

FLUSH PRIVILEGES;
```

## Backups

**Backup base de datos:**

```apache
mysqldump -u <usr> -p <db> > fichero_backup.sql
```

Con tiempo:

```apache
database_name-$(date +%Y%m%d).sql
```

**Backup ficheros:**

Comprimir: `sudo tar -cvzf backup.tar.gz > /home/chistes/backups`
Descomprimir: `sudo tar -xvzf /home/chistes/backupsbackup.tar.gz > dondesea`

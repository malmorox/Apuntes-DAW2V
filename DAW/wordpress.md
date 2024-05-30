# WORDPRESS

## [Instalación de Wordpress](https://es.wordpress.org/download/) <span style="font-size: xx-small;"> (En el cliente) </span>

### ¿Qué necesitamos?

Necesitaremos PHP 7.4 o superior y  MySQL 5.7-MariaDB 10.4 o superior.

```apache
$=> sudo apt install mariadb-server
```

```bash
MariaDB [(none)]> CREATE DATABASE chistes;
Query 0K, 1 row affected (0.000 sec)

MariaDB [(none)]> CREATE USER 'chistes'@'localhost' IDENTIFIED BY 'chistes';
Query OK, O rows affected (0.002 sec)

MariaDB [(none)]> GRANT ALL PRIVILEGES on marcos.* TO 'chistes'@'localhost';
Query OK, 0 rows affected (0.009 sec)

MariaDB [(none)]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.000 sec)
```

libapache2-mod-php

```apache
$=> sudo apt install php-mysql
```

### Configuracion de usuario

```apache
$=> sudo adduser chistes
        New password: chistes
        Retype new password: chistes
        Is the information correct? [Y/n] Y
```

```apache
$=> sudo mkdir /var/www/chistes
```

```apache
$=> sudo chown chistes:chistes /var/www/chistes
```

### Configuración de sitio con apache2

```apache
$=> sudo nano /etc/apache2/sites-available/005-chistes.conf
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

### FileZilla

- Descomprimimos el Wordpress omprimido que hemos descargado
- Nos conectamos con FileZilla al servidor y subimos todos los archivos de la carpeta descomprimida a /var/www/marcos
# Configuración de Apache2

## Paso 1: Instalar Apache

Comencemos actualizando el índice de paquetes locales para que reflejen los últimos cambios anteriores:
```apache
sudo apt update
```
Instalamos el paquete de apache2:
```apache
sudo apt install apache2
```


# Servicios

## MKDOCS

### ¿Qué necesitamos?
Necesitaremos pip, el gestor de paquetes de Python, para descargar el paquete:
```apache
$=> sudo apt install python3-pip
```

### Instalación de MkDocs
```apache
$=> pip install mkdocs
```

### Despliegue de MkDocs
Creamos un nuevo proyecto MkDocs en el directorio "/var/www/laura":
```apache
$=> sudo mkdocs new /var/www/laura
```
```apache
$=> cd /var/www/laura
```
Este comando construye el sitio web de MkDocs en el directorio actual:
```apache
$=> sudo mkdocs build
```

### Configuración de sitio con apache2
Abrimos y editamos el archivo de configuración de Apache para el sitio "laura.es":
```apache
$=> sudo nano /etc/apache2/sites-available/004-laura.conf
```
```apache
<VirtualHost *:80>
    ServerName laura.es
    DocumentRoot /var/www/laura
    <Directory "/var/www/laura/site">
        Options -Indexes
        AllowOverride None
        Order deny,allow
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/laura.error.log
    CustomLog ${APACHE_LOG_DIR}/laura.access.log combined
</VirtualHost>
```
Habilitamos el archivo de configuración del sitio web:
```apache
$=> sudo a2ensite 004-laura.conf
```
Recargamos Apache para aplicar los cambios de configuración:
```apache
$=> sudo systemctl reload apache2
```

## DOKUWIKI



## PHPDOCS
#### [Documentación de instalación](https://docs.phpdoc.org/3.0/guide/getting-started/installing.html#installation)
```apache
$=> sudo apt install php-mbstring
```
```apache
$=> mkdir myDocs
$=> cd myDocs
```
```apache
$=> wget https://phpdoc.org/phpDocumentor.phar
```
//Carpetapersonal (home)
```apache
$=> chmod u+x phpDocumentor.phar
```
Hay que tener instalado el php-cli
```apache
$=> php ./phpDocumentor.phar 
```
```apache
$=> ./phpDocumentor.phar run-d rutadelsrc/-trutadondequeremoseldocs/docs/
```

## WORDPRESS

#### [Instalación de Wordpress](https://es.wordpress.org/download/) <span style="font-size: xx-small;"> (En el cliente) </span>

### ¿Qué necesitamos?
Necesitaremos PHP 7.4 o superior y  MySQL 5.7/MariaDB 10.4 o superior.

Lo haremos con MySQL:

```apache
$=> sudo apt install mysql_secure_installation
```
```bash
Switch to unix_socket authentication [Y/n] => Y
Change the root password? [Y/n] => n
Remove anonymous users? [Y/n] => Y
Disallow root login remotely? [Y/n] => Y
Remove test database and access to it? [Y/n] => Y
Reload privilege tables now? [Y/n] => Y
```
```apache
$=> sudo apt install php-mysql
```

### Configuracion de usuario
```apache
$=> sudo adduser marcos
        New password: marcos
        Retype new password: marcos
        Is the information correct? [Y/n] Y
```
```apache
$=> sudo mkdir /var/www/marcos
```
```apache
$=> sudo chown marcos:marcos /var/www/marcos
```

### Configuración de sitio con apache2
```apache
$=> sudo nano /etc/apache2/sites-available/005-marcos.conf
```
```apache
<VirtualHost *:80>
    ServerName marcos.es
    DocumentRoot /var/www/marcos
    
    AssignUserId marcos marcos

    ErrorLog ${APACHE_LOG_DIR}/marcos.error.log
    CustomLog ${APACHE_LOG_DIR}/marcos.access.log combined
</VirtualHost>
```
```apache
$=> sudo a2ensite 005-marcos.conf
```
```apache
$=> sudo systemctl reload apache2
```

### FileZilla

- Descomprimimos el Wordpress omprimido que hemos descargado
- Nos conectamos con FileZilla al servidor y subimos todos los archivos de la carpeta descomprimida a /var/www/marcos

## MOODLE

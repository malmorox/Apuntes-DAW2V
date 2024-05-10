# Git y Github

### ¿Qué necesitamos?
Necesitaremos git:
```apache
$=> sudo apt install git
```

## Repositorio en local con Git

###  Crear el repositorio git

Comenzamos creando el directorio donde quereamos inicicalizar el repositorio git:
```apache
$=> mkdir repositorio
$=> cd repositorio/
```
Inicializamos git:
```apache
$=> git init
```
### Borrar repositorio git

```apache
$=> rm -r .git/
```
### Introducir datos usuario al git local

```apache
$=> git config--global user.name nombre
$=> git config--global user.mail nombre@daw.git
```

### Comandos para guardar cambios

 $=> git status // Estado del repositorio
 $=> git add // Añadir al Stage
 $=> git commit // Hecha la foto

# Servicios

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

## PHPDOCS
#### [Documentación de instalación](https://docs.phpdoc.org/3.0/guide/getting-started/installing.html#installation)
```apache
$=> sudo apt install php8.1-cli
```
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

```php
<?php
    class Perro {
        public $nombre;
        /**
        * Hace que el perro ladre
        *
        * @param integer $veces indica el número de ladridos
        * @param string $tipo indica el ladrido por defecto “Guau”
        *
        */
        public ladra(integer $veces, string $tipo="Guau")  {

        }
    }
?>
```
```apache
$=> chmod a+x phpDocumentor.phar
```
```apache
$=> mv phpDocumentor.phar /usr/local/bin/phpDocumentor 
```


```apache
$=> phpDocumentor run -d rutadelsrcconclases/ -t rutadondequeremoseldocs/docs/
```
Si nos da error, deberemos instalar PHP XML, ya que puede ser que no venga instalado:
```apache
$=> sudo apt install php8.1-xml
```

## WORDPRESS

#### [Instalación de Wordpress](https://es.wordpress.org/download/) <span style="font-size: xx-small;"> (En el cliente) </span>

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

## MOODLE

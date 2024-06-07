# MOODLE

### ¿Qué necesitamos?

Necesitaremos PHP 7.4 o superior y  MySQL 5.7-MariaDB 10.4 o superior.

`sudo apt install php-gd php-curl php-intl php-xml php-xmlrpc php-json php-zip`

**Software adicional**

``
sudo apt install graphviz aspell ghostscript clamav php-pspell php-ldap php-soap php-mbstring
``

**Reiniciar apache2 para que se apliquen los modulos**

`sudo service apache2 restart`

### Configuracion de usuario

```apache
$=> sudo adduser moodle
        New password: moodle
        Retype new password: moodle
        Is the information correct? [Y/n] Y
```

```apache
$=> sudo mkdir /var/www/moodle
$=> sudo chown moodle:moodle /var/www/moodle
```

### Descargar Moodle <span style="font-size: small;"> (En el cliente) </span>

```
cd Escritorio/
```

#### **Coger código de Moodle:**

```
sudo git clone git://git.moodle.org/moodle.git
```

#### **Copiar el contenido en el virtualhost** (En el servidor)

```
sudo mkdir /var/www/moodle
```

Con el FileZilla subimos los archivos descargados en el cliente a la carpeta creada del servidor

#### **Crear área de datos:**

Deberemos crear un directorio donde se guardarán los datos de nuestro sitio web de Moodle. Le podemos llamar como queramos, pero si le cambiamos el nombre deberemos cambiarlo más tarde en la instalación web

```
mkdir /var/www/moodledata
```

## Archivo de configuracion de Apache

```apache
<VirtualHost *:80>
    ServerName moodle.es
    ServerAdmin moodle@localhost
    DocumentRoot /var/www/moodle

    ErrorLog ${APACHE_LOG_DIR}/moodle.error.log
    CustomLog ${APACHE_LOG_DIR}/moodle.access.log combined
</VirtualHost>
```

```apache
sudo a2ensite 006-moodle.conf
sudo systemctl restart apache2
```

### Crear base de datos

```SQL
mysql -u root -p
CREATE DATABASE moodle default character set utf8;
CREATE USER 'moodleuser'@'localhost' IDENTIFIED BY 'contra';
GRANT ALL ON moodle.* to moodleuser@localhost identified by 'contra';
exit;
```

### En la configuración de la BBDD (En el navegador)

Database port: 3306

### Instalación de Moodle <span style="font-size: small;"> (En el navegador) </span>

En el cliente abrir el navegador:

`
http://ipserver.dominio.moodle.es/
`

**Para finalizar:**

```
chmod 0755 /var/www/moodle
```

#### Error a solucionar que sale en la instalación del navegador

***max_input_vars = 5000*** en el fichero /etc/php/{version}/apache2/php.ini

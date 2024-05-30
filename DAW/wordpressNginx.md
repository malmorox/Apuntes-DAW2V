# Instalar Wordpress en Nginx

## Instalar extensiones de PHP

```
sudo apt install php-curl php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip
```

## Crear DataBase

**Entrar al mySQL**

```
sudo mysql

mysql -u root -p
```

**Crear base de datos**

```sql
CREATE DATABASE nombredb DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

**Crear user**

```sql
CREATE USER 'nombreuser'@'localhost' IDENTIFIED BY 'password';
```

**Dar permisos al user**

```sql
GRANT ALL ON nombredb.* TO 'wordpressuser'@'localhost';
EXIT;
```

## Configurar Nginx

**Editar archivo Nginx**

```
sudo vim /etc/nginx/sites-available/nombreArchivo
```

**Ver errores de sintaxis**

```
sudo nginx -t
```

**Reiniciar nginx**

```
sudo systemctl reload nginx
```

## Descargar WordPress

**Descarga del comprimido**

```
cd /tmp
curl -LO https://wordpress.org/latest.tar.gz
```

**Descomprimir**

```
tar xzvf latest.tar.gz
```

**Crear wp-config.php**

```
cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
```

**Mover los archivos de wordpress**

```
sudo cp -a /tmp/wordpress/. /var/www/dirName/wordpress
```

**Asignar a www-data**

```
sudo chown -R www-data:www-data /var/www/your_domain/wordpress
```

## Modificar el archivo de configuración de WordPress (No es esencial)

**Secret-Keys** *(En el cliente)*

```
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```

## Configuración desde la web

```
http://server_domain_or_IP/wordpress
```
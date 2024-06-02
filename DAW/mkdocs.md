# MKDOCS

## ¿Qué necesitamos?

Necesitaremos pip, el gestor de paquetes de Python, para descargar el paquete:

```apache
sudo apt install python3-pip
```

### Instalación de MkDocs

```apache
pip install mkdocs
```

### Despliegue de MkDocs

Creamos un nuevo proyecto MkDocs en el directorio "/var/www/laura":

```apache
mkdocs new /var/www/laura
```

```apache
cd /var/www/laura
```

Este comando construye el sitio web de MkDocs en el directorio actual:

```apache
sudo mkdocs build
```

### Configuración de sitio con apache2

Abrimos y editamos el archivo de configuración de Apache para el sitio "laura.es":

```apache
sudo nano /etc/apache2/sites-available/004-laura.conf
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
sudo a2ensite 004-laura.conf
```

Recargamos Apache para aplicar los cambios de configuración:

```apache
sudo systemctl reload apache2
```

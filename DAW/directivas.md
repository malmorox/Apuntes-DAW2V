# DIRECTIVAS

- **Requiere all**
  - *Require all granted*: Permite el acceso a todos sin restricciones.
  - *Require all denied*: Niega el acceso a todos sin excepción.

- **Require ip**
  - *Requireip 192.168.1.0/24*: Permite el acceso solo desde las direcciones IP en la subred especificada.

- **Require host**
  - *Require host example.com*: Permite el acceso solo desde los hosts especificados.

- **Require user**
  - *Require user username*: Permite el acceso solo a los usuarios especificados. Se requiere configurar la autenticación.

## Ejemplos

### Ejercicio htaccess del T1

```apache
<VirtualHost *:80>
        ServerName practica.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/practica


        ErrorLog ${APACHE_LOG_DIR}/practica.error.log
        CustomLog ${APACHE_LOG_DIR}/practica.access.log combined

        <Directory /var/www/practica>
                        #Raiz
                        #no se permite listar el contenido del directorio
                        Options -Indexes
                        #no se permite sobreescribir la configuración con htaccess
                        AllowOverride None
        </Directory>
        <Directory /var/www/practica/conf1>
                        #Conf1
                        #el fichero que se visita por defecto al pedir su raiz es a.html
                        DirectoryIndex a.html
        </Directory>
        <Directory /var/www/practica/conf2>
                        #Conf2
                        #Se permite configurar con htacces
                        AllowOverride All
                        #Se permite listar los directorios
                        Options Indexes
        </Directory>
        <Directory /var/www/practica/conf3>
                        #Conf3
                        #Se permite configurar con htacces
                        AllowOverride All
                        #se permite listar directorios (configuración en htaccess)
                        Options Indexes
                        #Se pide usuario y contraseña examen, examen (configuración en htaccess)
                        AuthType Basic
                        AuthName "Area restringida"
                        #Guardar en /etc/apache2/.htpasswd
                        AuthUserFile /etc/apache2/.htpasswd
                        Require valid-user
        </Directory>
        # Mostrar el estado del servidor
        <Location "/conf3/estado">
                SetHandler server-status
                Require local
        AuthType Basic
                        AuthName "Area restringida"
                        #Guardar en /etc/apache2/.htpasswd
                        AuthUserFile /etc/apache2/.htpasswd
                        Require valid-user
        </Location>
</VirtualHost>
```

# DIRECTIVAS

Las directivas de seguridad en Apache2 son esenciales para controlar el acceso a los recursos del servidor web. A continuación, se describen las principales directivas de seguridad que se pueden utilizar dentro de una sección `<Directory>`.

## AllowOverride

Controla qué directivas pueden ser sobrescritas en archivos `.htaccess`.

```apache
<Directory "/var/www/html">
    AllowOverride All
</Directory>
```

## Require

Controla el acceso a los recursos especificados. Se pueden utilizar diferentes parámetros para configurar el acceso, como all, ip, host, env, user, y group.

- **Requiere all**
  - *Require all granted*: Permite el acceso a todos sin restricciones.
  - *Require all denied*: Niega el acceso a todos sin excepción.

- **Require ip**
  - *Require ip 192.168.1.0/24*: Permite el acceso solo desde las direcciones IP en la subred especificada.

- **Require host**
  - *Require host example.com*: Permite el acceso solo desde los hosts especificados.

- **Require user**
  - *Require user username*: Permite el acceso solo a los usuarios especificados. Se requiere configurar la autenticación.

### Ejemplos de Require:

Permitir acceso solo desde una dirección IP específica:

```apache
<Directory "/var/www/html">
    Require ip 192.168.1.0/24
</Directory>
```
Permitir acceso solo a usuarios autenticados:

```apache
<Directory "/var/www/html/private">
    AuthType Basic
    AuthName "Restricted Area"
    AuthUserFile /etc/apache2/.htpasswd
    Require valid-user
</Directory>
```


## Options

Configura varias opciones para el directorio, como Indexes, FollowSymLinks, Includes, y más.

```apache
<Directory "/var/www/html">
    Options Indexes FollowSymLinks
</Directory>
```

## Order, Allow, Deny <span style="font-size: small;"> (Obsoletas en Apache 2.4 y posteriores) </span>

En versiones anteriores a Apache 2.4, se utilizaban Order, Allow y Deny para controlar el acceso. En Apache 2.4 y posteriores, se recomienda usar Require.

```apache
<Directory "/var/www/html">
    Order allow,deny
    Allow from all
</Directory>
```

## AuthType y AuthName

Estas directivas se utilizan para configurar la autenticación básica.

```apache
<Directory "/var/www/html/private">
    AuthType Basic
    AuthName "Restricted Area"
    AuthUserFile /etc/apache2/.htpasswd
    Require valid-user
</Directory>
```

## AuthBasicProvider

Especifica los proveedores de autenticación para el tipo de autenticación básica.

```apache
<Directory "/var/www/html/private">
    AuthType Basic
    AuthName "Restricted Area"
    AuthBasicProvider file
    AuthUserFile /etc/apache2/.htpasswd
    Require valid-user
</Directory>
```

## AuthUserFile y AuthGroupFile

Especifica la ubicación de los archivos de usuarios y grupos para la autenticación básica.

```apache
<Directory "/var/www/html/private">
    AuthType Basic
    AuthName "Restricted Area"
    AuthUserFile /etc/apache2/.htpasswd
    AuthGroupFile /etc/apache2/.htgroup
    Require group admin
</Directory>
```

## SSLRequire

Permite controlar el acceso basado en atributos SSL.

```apache
<Directory "/var/www/html/secure">
    SSLRequireSSL
</Directory>
```

## Satisfy

Controla si una solicitud debe cumplir con todos los criterios de acceso o solo uno de ellos.

```apache
<Directory "/var/www/html">
    Satisfy any
    Require ip 192.168.1.0/24
    Require user valid-user
</Directory>
```



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

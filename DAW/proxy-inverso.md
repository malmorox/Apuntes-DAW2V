# PROXY INVERSO

Un proxy inverso es un servidor que intercepta las peticiones de los clientes a un servidor y redirige esas peticiones a uno o más servidores en el backend. Apache2 puede actuar como proxy inverso para balancear la carga, mejorar la seguridad y gestionar el tráfico de red.


Módulos adicionales a instalar para funcionar como un proxy inverso:

```apache
sudo a2enmod proxy_http
sudo systemctl restart apache2
```

## Configuración de Apache2

**Crear un nuevo archivo de configuración:**

```apache
sudo micro /etc/apache2/sites-available/proxy-inverso.conf
```

**Configurar el Proxy Inverso**

Dentro de este archivo, define la configuración básica para el proxy inverso:

```apache
<VirtualHost *:80>
        ServerName proxy-inverso.es

        ProxyPass "/" "http://127.0.0.1:8050/"
        ProxyPassReverse "/" "http://127.0.0.1:8050/"

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

## Ejemplos donde usar

### Redirigiendo otro dominio de nuestro /etc/hosts:

Entramos en /etc/hosts y vemos que existe un nombre de dominio que contiene un PHPDocs, por ejemplo:

`192.168.56.2    phpdocs.es`

Creamos un nuevo archivo de configuración para nuestro sitio:
```apache
sudo nano /etc/apache2/sites-available/proxy-inverso.conf
```

Hacemos el proxy inverso al dominio de phpdocs.es que esta sirviendo nuestro servidor:

```apache
<VirtualHost *:80>
        ServerName proxy-inverso.es

        ProxyPass "/" "http://phpdocs.es/"
        ProxyPassReverse "/" "http://phpdocs.es/"

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Habilitamos el sitio y reiniciamos apache:

```apache
sudo a2ensite proxy-inverso.conf
sudo systemctl reload apache2
```

Ahora, cuando accedas a http://proxy-inverso.es, Apache2 redirigirá las solicitudes a http://phpdocs.es (donde encontraras un PHPDocs), que se resolverá como 192.168.56.2 gracias a la entrada en /etc/hosts.

hay que tenter en cuneta que al estar usando un nombre de dominio debe estar registrado en el /etc/hosts del servidor si no no encontrá la pagina a la que intentas redirigir (en este ejemplo se debe registar phpdocs.es)


### Usando un puerto especifico con IP:



```apache
<VirtualHost *:80>
        ServerName proxy-inverso.es

        ProxyPass "/" "http://192.168.56.2:8001/"
        ProxyPassReverse "/" "http://192.168.56.2:8001/"

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

```apache
<VirtualHost *:80>
        ServerName proxy-inverso.es

        ProxyPass "/" "http://127.0.0.1:8001/"
        ProxyPassReverse "/" "http://127.0.0.1:8001/"

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Habilitamos el sitio y reiniciamos apache:

```apache
sudo a2ensite proxy-con-etc-hosts.conf
sudo systemctl reload apache2
```

Ahora, cuando accedas a http://proxy-inverso.es, Apache2 redirigirá las solicitudes a http://phpdocs.es (donde encontraras un PHPDocs), que se resolverá como 192.168.56.2 gracias a la entrada en /etc/hosts.
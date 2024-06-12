# Configuracion servidor + Apache2

## Configurar IP y entrar al Servidor

- File Netplan => /etc/netplan/00-installer-config.yaml

```yaml
 # This is the network config written by 'subiquity'

network:
  ethernets:
    enp0s3:
      dhcp4: false
      addresses: [192.168.14.200+numPC/24] # IP del pc
      gateway4: 192.168.14.100
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
  version: 2

```

- Aplicar los cambios:

```apache
netplan apply
```

## DNS local usando el archivo hosts

```bash
127.0.0.1       localhost
127.0.1.1       cliente
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
# servidores
192.168.1.158 wordpress.dmm.examen
192.168.1.158 docs.dmm.examen
192.168.1.158 directivas.dmm.examen
```

## Comprimidos

```apache
unzip archivo
```

**Crear Tar:**

```apache
tar -cvf 
```

**Crear tar.gz / tgz:**

```apache
tar -cvzf archivo.tgz carpeta-a-comprimir/
```

**Descomprimir:**

```apache
tar -xvf
```

## Hosting Apache2

**Añadir servidor con nombre en el archivo hosts:**

```apache
vim /etc/hosts
```

```apache
# Servidores locales
192.168.137.208 miserver.es
```

**Instalar el openssh-client:**

```apache
sudo apt-get install openssh-client
```

**Entrar al servidor:**

ssh <user@miserver.es>

## Configuración de APACHE2

**Archivos y Rutas:**

```apache
/var/www/dirName => directorio del hosting

/etc/apache2/apache2.conf => Archivo de configuración de apache2

/etc/apache2/sites-available/000-default.conf => archivo de configuración del host por defecto
```

**Archivo de configuración de VirtualHost:**

```apache
<VirtualHost *:80>
  ServerName web.com # Nombre del dominio, modificar el host del client con la IP y dominio
  ServerAdmin correo@web.com # Correo del administrador
  DocumentRoot /var/www/nombreDir/* # Directorio de virtualhost
  ErrorLog ${APACHE_LOG_DIR}/error.log # Archivo de errores
  CustomLog ${APACHE_LOG_DIG}/access.log combined # Archivo de accesos
</Virtualhost>
```

## Directivas

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

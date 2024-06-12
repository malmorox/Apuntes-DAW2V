# DOCKER

**Imagen =** es un archivo de pocos bytes cuyo objetivo es ser ejecutado dentro de un container, basicamente una iso con poco software.
**Contenedor=** Es cuando cojemos una imágen y la metemos en docker

## Instalación de Docker Engine en Ubuntu Server

**Requisitos de Sistema Operativo**

- Ubuntu Mantic 23.10
- Ubuntu Jammy 22.04 (LTS)
- Ubuntu Focal 20.04 (LTS)

**Desinstalar Versiones antiguas**

```bash
 for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

**Instalación y cofiguraión de los repositorios oficiales de Docker**

**Añadir la clave GPG oficial de Docker**

Instalar los paquetes necesarios:

```apache
sudo apt-get update
sudo apt-get install ca-certificates curl
```

Añadir la clave GPG:

```apache
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Añadir el repositorio:

**Si se utiliza Ubuntu:**

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

**si se utiliza algun derivado de ubuntu como linux mint:**

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$UBUNTU_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Instalar la ultima versión de Docker:

```apache
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Verificar que este bien instalado

```apache
sudo docker run hello-world
```

## Usar docker sin ser el admin

```apache
sudo groupadd docker
sudo usermod -aG docker userName
```

## Chuleta

<img src="./img/cheatDocker.png">

# Crear Docker

## Imagenes necesarias

### Apache

**Descargar la imagen:**

```apache
docker pull httpd
```

**Servir archivos con esta imagen:**

```apache
docker run -d --name my-apache-app -p 127.0.0.1:8080:80 -v /contenido/:/usr/local/apache2/htdocs/ httpd
```

**Explicacion del comando:**


<span style="color:red;">-d</span>: Ejecuta el contenedor en segundo plano (detached mode), permitiendo que la terminal quede libre para otros comandos.

<span style="color:red;">--name</span> <span style="color:cyan;">my-apache-app</span>:
Asigna el nombre  <span style="color:cyan;">my-apache-app</span> al contenedor. Esto facilita la gestión y referencia del contenedor por su nombre.

<span style="color:red;">-p</span>  <span style="color:cyan;">127.0.0.1</span>:<span style="color:violet;">8050</span>:<span style="color:orange;">80</span>
  * <span style="color:cyan;">127.0.0.1</span>:<span style="color:violet;">8050</span>: El puerto <span style="color:violet;">8050</span> en <span style="color:cyan;">localhost</span> del host.
  * <span style="color:orange;">80</span>: El puerto <span style="color:orange;">80</span> dentro del contenedor, donde Apache HTTP Server escucha.

<span style="color:red;">-v</span>: <span style="color:green;">/mkdocs-docker/</span>:<span style="color:cyan;">/usr/local/apache2/htdocs/</span>
Monta un volumen, enlazando un directorio del host con un directorio del contenedor:
  * <span style="color:green;">/mkdocs-docker/</span>: El directorio en el host (directorio actual relativo) que contiene los archivos que quieres servir.
  * <span style="color:cyan;">/usr/local/apache2/htdocs/</span>: El directorio en el contenedor donde Apache sirve los archivos.

<span style="color:red;">httpd</span>:
Especifica la imagen a utilizar para crear el contenedor:

### Php + Apache

**Descargar la imagen:**

```apache
docker pull php:8.3.8RC1-apache-bullseye
```

**Servir archivos con esta imagen:**

```apache
docker run -d --name my-apache-app -p 127.0.0.1:8080:80 -v /contenido/:/var/www/html php:8.3.8RC1-apache-bullseye
```

**Explicacion del comando:**


<span style="color:red;">-d</span>: Ejecuta el contenedor en segundo plano (detached mode), permitiendo que la terminal quede libre para otros comandos.

<span style="color:red;">--name</span> <span style="color:cyan;">my-apache-app</span>:
Asigna el nombre  <span style="color:cyan;">my-apache-app</span> al contenedor. Esto facilita la gestión y referencia del contenedor por su nombre.

<span style="color:red;">-p</span>  <span style="color:cyan;">127.0.0.1</span>:<span style="color:violet;">8050</span>:<span style="color:orange;">80</span>
  * <span style="color:cyan;">127.0.0.1</span>:<span style="color:violet;">8050</span>: El puerto <span style="color:violet;">8050</span> en <span style="color:cyan;">localhost</span> del host.
  * <span style="color:orange;">80</span>: El puerto <span style="color:orange;">80</span> dentro del contenedor, donde Apache HTTP Server escucha.

<span style="color:red;">-v</span>: <span style="color:green;">/mkdocs-docker/</span>:<span style="color:cyan;">/var/www/html</span>
Monta un volumen, enlazando un directorio del host con un directorio del contenedor:
  * <span style="color:green;">/mkdocs-docker/</span>: El directorio en el host (directorio actual relativo) que contiene los archivos que quieres servir.
  * <span style="color:cyan;">/var/www/html</span>: El directorio en el contenedor donde Apache sirve los archivos.

<span style="color:red;">php:8.3.8RC1-apache-bullseye</span>:
Especifica la imagen a utilizar para crear el contenedor:

### Nginx

**Descargar la imagen:**

```apache
docker pull nginx
```

**Servir archivos con esta imagen:**

```apache
docker run -d --name my-apache-app -p 127.0.0.1:8080:80 -v /contenido/:/usr/share/nginx/html:ro nginx
```

**Explicacion del comando:**


<span style="color:red;">-d</span>: Ejecuta el contenedor en segundo plano (detached mode), permitiendo que la terminal quede libre para otros comandos.

<span style="color:red;">--name</span> <span style="color:cyan;">my-nginx-app</span>:
Asigna el nombre  <span style="color:cyan;">my-nginx-app</span> al contenedor. Esto facilita la gestión y referencia del contenedor por su nombre.

<span style="color:red;">-p</span>  <span style="color:cyan;">127.0.0.1</span>:<span style="color:violet;">8050</span>:<span style="color:orange;">80</span>
  * <span style="color:cyan;">127.0.0.1</span>:<span style="color:violet;">8050</span>: El puerto <span style="color:violet;">8050</span> en <span style="color:cyan;">localhost</span> del host.
  * <span style="color:orange;">80</span>: El puerto <span style="color:orange;">80</span> dentro del contenedor, donde Nginx escucha.

<span style="color:red;">-v</span>: <span style="color:green;">/mkdocs-docker/</span>:<span style="color:cyan;">/usr/local/nginx/html</span>:<span style="color:violet;">ro</span>
Monta un volumen, enlazando un directorio del host con un directorio del contenedor:
  * <span style="color:green;">/mkdocs-docker/</span>: El directorio en el host (directorio actual relativo) que contiene los archivos que quieres servir.
  * <span style="color:cyan;">/usr/local/nginx/html/</span>: El directorio en el contenedor donde Nginx sirve los archivos.
  * <span style="color:violet;">ro</span>: (read only) runea la imagen solo para lectura

<span style="color:red;">nginx</span>:
Especifica la imagen a utilizar para crear el contenedor

## Configuracion extra

como hemos asignado la ip <span style="color:cyan;">127.0.0.1</span> a la que solo se podria acceder desde la maquina en la que se ejecuta el contenedor, para poder acceder desde otra maquina vamos a tener que configurar un proxy inverso desde Apache o Nginx (ahora solo lo vamos a hacer con Apache)

### Ejemplo de configuracion del proxy inverso

```apache
<VirtualHost *:80>
        ServerName tu-nombre-dominio.es

        ProxyPass "/" "http://127.0.0.1:8050/"
        ProxyPassReverse "/" "http://127.0.0.1:8050/"


        ErrorLog ${APACHE_LOG_DIR}/nginx-php.error.log
        CustomLog ${APACHE_LOG_DIR}/nginx-php.access.log combined
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

### IMPORTANTE que la ip y el puerto en el ProxyPass/ProxyPassReverse sea la misma que habiamos puesto en el comando de docker (<span style="color:cyan;">127.0.0.1</span>:<span style="color:violet;">8050</span>)
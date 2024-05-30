# Nginx Básico

## Instalar Nginx

```
sudo apt update
sudo apt install nginx
```

## Ajustar el firewall

```
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status
```

## Comprobar el estado

```
systemctl status nginx
```

## Manegar Nginx Process

**Para y empezar servicio**

```
sudo systemctl stop nginx
sudo systemctl start nginx
```

**Para y empezar servicio al mismo tiempo**

```
sudo systemctl restart nginx
```

**Recargar, sin para conexiones**

```
sudo systemctl reload nginx
```

**Desactivar Nginx**

```
sudo systemctl disable nginx
```

**Permitir Nginx vuelva a iniciar al boot**

```
sudo systemctl enable nginx
```

## Archivos de Nginx 

**/etc/nginx/sites-available/archivo**

```nginx
server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

**Para permitir que el servidor funcione**

```
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

**/etc/nginx/nginx.conf**

```nginx
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
```

**Restart**

```
sudo systemctl restart nginx
```

# Nginx Proxy inverso

## Crear virtual host

```
sudo vim /etc/nginx/sites-available/midominio.com.conf
```

```nginx
server {
  #Escucha en el puerto 80, ipv4.
  listen 80; 
  
  #Aquí deberás introducir el nombre de tu dominio.
  server_name midominio.com;

  access_log            /var/log/nginx/midominio.com.access.log;

  location / {
      #La configuración del proxy.
      proxy_pass http://localhost:4000/;
  }
}
```

```
sudo systemctl reload nginx
```


## CHAT GPT EXAMPLE

Un proxy inverso en Nginx se utiliza para redirigir las solicitudes de los clientes a uno o más servidores backend, y luego enviar las respuestas de esos servidores de vuelta al cliente. Aquí tienes un ejemplo básico de configuración de un proxy inverso en Nginx:

Supongamos que quieres configurar Nginx como un proxy inverso para redirigir las solicitudes HTTP entrantes a un servidor backend que se ejecuta en la dirección IP 192.168.1.100 en el puerto 8080.

```nginx
server {
    listen 80;
    server_name tunombredehost.com;

    location / {
        proxy_pass http://192.168.1.100:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

En esta configuración:

- `listen 80;`: Esto hace que Nginx escuche las solicitudes en el puerto 80, que es el puerto predeterminado para HTTP.
- `server_name tunombredehost.com;`: Esto especifica el nombre de host al que Nginx responderá. Debes reemplazar "tunombredehost.com" con tu propio nombre de dominio.
- `location / {...}`: Esta directiva establece las reglas para manejar las solicitudes entrantes. En este caso, todas las solicitudes son manejadas por el bloque `location /`.
- `proxy_pass http://192.168.1.100:8080;`: Esta línea indica que Nginx debe redirigir todas las solicitudes a la dirección IP 192.168.1.100 en el puerto 8080.
- Las líneas `proxy_set_header` se utilizan para configurar las cabeceras HTTP que se enviarán al servidor backend.

Recuerda que necesitarás reiniciar o recargar Nginx después de hacer cambios en la configuración para que surtan efecto:

```
sudo systemctl restart nginx
```

Es importante tener en cuenta que esta es solo una configuración básica y que puedes personalizarla según tus necesidades específicas, como la gestión de rutas, balanceo de carga, SSL, entre otros.
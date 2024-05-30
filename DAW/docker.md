# Docker

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

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
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

## Instalar Docker Image

**Hub Docker**

```apache
sudo docker pull $nombre
$nombre = imagen (ej.: httpd)
```

# Crear Docker

```apache
 git clone https://github.com/docker/getting-started-app.git
```

```
├── getting-started-app/
│ ├── package.json
│ ├── README.md
│ ├── spec/
│ ├── src/
│ └── yarn.lock
```

## Build the app´s image

**Directorio en el que trabajamos**

```apache
cd /path/to/getting-started-app
```

**Crear archivo**

```apache
touch Dockerfile
```

**Contenido del fichero**

```docker
# syntax=docker/dockerfile:1

FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

**Comandos para el build**

```apache
docker build -t getting-started .
```

**Arrancar el contenedor**

```apache
docker run -dp 127.0.0.1:3000:3000 getting-started
```

**Listar dockers**

```apache
docker ps
```

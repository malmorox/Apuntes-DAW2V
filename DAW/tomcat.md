# TOMCAT

## Instalar Java

```apache
sudo apt update
sudo apt install openjdk-11-jdk
java --version
```

## Creación de usuario TomCat en el Server

```apache
sudo useradd -m -U -d /opt/tomcat -s /bin/false tomcat
```

## Descargar TomCat

```apache
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.24/bin/apache-tomcat-10.1.24.tar.gz
```

## Descomprimir y llevar a la Carpeta de TomCat

```apache
sudo tar -xf apache-tomcat-10.1.24.tar.gz -C /opt/tomcat/

```

## Crear enlace Simbolico

```apache
sudo ln -s /opt/tomcat/apache-tomcat-10.1.24 /opt/tomcat/latest 

```

## Dar control al usuario tomcat

```apache
sudo chown -R tomcat: /opt/tomcat
```

## Dar Ejecución a los TomCat

```apache
sudo sh -c 'chmod +x /opt/tomcat/latest/bin/*.sh'
```

## Crear Fichero SystemD Unit

```apache
sudo micro /etc/systemd/system/tomcat.service
```

**Contenido del archivo:**

```ini
[Unit]
Description=Tomcat 10 servlet container
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom -Djava.awt.headless=true"

Environment="CATALINA_BASE=/opt/tomcat/latest"
Environment="CATALINA_HOME=/opt/tomcat/latest"
Environment="CATALINA_PID=/opt/tomcat/latest/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/latest/bin/startup.sh
ExecStop=/opt/tomcat/latest/bin/shutdown.sh

[Install]
WantedBy=multi-user.target
```

## DAEMON TIME

```apache
sudo systemctl daemon-reload
sudo systemctl enable --now tomcat
sudo systemctl status tomcat
```

```apache
sudo systemctl start tomcat
sudo systemctl stop tomcat
sudo systemctl restart tomcat
```

## Configuración del Firewall

```apache
sudo ufw allow 8080/tcp
```

## Configuración TomCat Web Management Interface

```apache
sudo micro /opt/tomcat/latest/conf/tomcat-users.xml
```

**Contenido del Fichero:**

```xml
<tomcat-users>
<!--
    Comments
-->
   <role rolename="admin-gui"/>
   <role rolename="manager-gui"/>
   <user username="admin" password="admin_password" roles="admin-gui,manager-gui"/>
</tomcat-users>
```

**Manger app:**

```apache
sudo micro /opt/tomcat/latest/webapps/manager/META-INF/context.xml
```

**Host Maneger App:**

```xml
<Context antiResourceLocking="false" privileged="true" >
<!--
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
</Context>
```

```apache
sudo systemctl restart tomcat
```

## Navegador Cliente

http://<your_domain_or_IP_address>:8080

>Ahora es posible que tomcat nos de algunos problemas de persmisos (que solo se pueda acceder desde la propia pagina) a la hora de acceder a ciertos sitios dentro de la propia pagina de tomcat, como por ejemplo la pagina de documentacion o de ejemplos.

>Para solucionar esto podemos configurar tomcat para que se ejecute en el localhost del servidor y hacer un proxi inverso desde apache.

**Entrar en superusuario y dirigirse a la carpeta:**

```bash
sudo su -
cd /opt/tomcat
```

**crear la carpeta de conf y el archivo server.xml dentro de esta:**

```bash
mkdir conf/
cd conf/
micro server.xml
```

**dentro del archivo de server introducir esta configuracion:**

```xml
<Connector 
    port="8080" 
    protocol="HTTP/1.1" 
    address="127.0.0.1"
    connectionTimeout="20000" 
    redirectPort="8443" 
  />
```

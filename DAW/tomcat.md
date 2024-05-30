
# Tomcat

## Instalar Java

```
sudo apt update
sudo apt install openjdk-11-jdk
java --version
```

## Creación de usuario TomCat en el Server

```
sudo useradd -m -U -d /opt/tomcat -s /bin/false tomcat
```

## Descargar TomCat

```
wget https://www-eu.apache.org/dist/tomcat/tomcat-10/v${VERSION}/bin/apache-tomcat-${VERSION}.tar.gz -P /tmp
```

## Descomprimir y llevar a la Carpeta de TomCat

```
sudo tar -xf /tmp/apache-tomcat-${VERSION}.tar.gz -C /opt/tomcat/
```

## Crear enlace Simbolico

```
sudo ln -s /opt/tomcat/apache-tomcat-${VERSION} /opt/tomcat/latest
```

## Dar control al usuario tomcat

```
sudo chown -R tomcat: /opt/tomcat
```

## Dar Ejecución a los TomCat

```
sudo sh -c 'chmod +x /opt/tomcat/latest/bin/*.sh'
```

## Crear Fichero SystemD Unit

```
sudo nano /etc/systemd/system/tomcat.service
```

**Contenido del archivo**

```apache
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

```
sudo systemctl daemon-reload
sudo systemctl enable --now tomcat
sudo systemctl status tomcat
```

```
sudo systemctl start tomcat
sudo systemctl stop tomcat
sudo systemctl restart tomcat
```

## Configuración del Firewall

```
sudo ufw allow 8080/tcp
```

## Configuración TomCat Web Management Interface

```
sudo nano /opt/tomcat/latest/conf/tomcat-users.xml
```

**Contenido del Fichero**

```apache
<tomcat-users>
<!--
    Comments
-->
   <role rolename="admin-gui"/>
   <role rolename="manager-gui"/>
   <user username="admin" password="admin_password" roles="admin-gui,manager-gui"/>
</tomcat-users>
```

**Manger app**

```
sudo nano /opt/tomcat/latest/webapps/manager/META-INF/context.xml
```

**Host Maneger App**

```apache
<Context antiResourceLocking="false" privileged="true" >
<!--
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
</Context>
```

```
sudo systemctl restart tomcat
```

## Navegador Cliente

http://<your_domain_or_IP_address>:8080

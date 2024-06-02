# Ejercicio 1

Vamos ha realizar paso a paso el ejercicio por sus apartados:

  * Crea un usuario en el servidor: scrordinaria. CAPTURA

  Abrimos Terminal y accedemos al server:

  ```
  ssh eracres@scrordinaria.es
  ```

  A continuación creamos el user:
  
  ```
  sudo adduser scrordinaria
  ```
    
  * Tanto el espacio web como el host estarán asociados al usuario, permisos
  más restrictivos.

  Para la modificacion de permisos tenemos que realizar el siguiente comando:

   1- Permisos para el host:

   ```
   sudo chmod -R 755 /var/www/scrordinaria/
   ```
   2- Permisos para el espacio web:

   ```
   <VirtualHost *:80>
       ServerName scrordinaria.t1.e1.es
       DocumentRoot /var/www/scrordinaria/t1/e1
   
       <Directory /var/www/scrordinaria/t1/e1>
           Options Indexes FollowSymLinks
           AllowOverride None
           Require all granted
       </Directory>
   
       ErrorLog ${APACHE_LOG_DIR}/jdlordinaria.t1.e1.es_error.log
       CustomLog ${APACHE_LOG_DIR}/jdlordinaria.t1.e1.es_access.log combined
   </VirtualHost>
   ```
      
  * Crea un espacio web en /var/www/jdlordinaria/t1/e1/ CAPTURA

    Creamos el directorio:
    
    ```
    sudo mkdir -p /var/www/jdlordinaria/t1/e1/
    ```
  
  * Crea un virtualhost jdlordinaria.t1.e1.es. CAPTURA

    Accedemos al fichero de hosts:

    ```
    sudo nano /etc/hosts
    ```

    Y especificamos nuestro host-local:

    ```
      GNU nano 6.2                       /etc/hosts                                 
      127.0.0.1       localhost
      127.0.1.1       eracres-VirtualBox
      
      # The following lines are desirable for IPv6 capable hosts
      ::1     ip6-localhost ip6-loopback
      fe00::0 ip6-localnet
      ff00::0 ip6-mcastprefix
      ff02::1 ip6-allnodes
      ff02::2 ip6-allrouters
      
      #Host locales
      172.20.0.2 scrordinaria.t1.e1.es
    ```
    
  * En él habrá dos directorios: info y doc
   
    Lo realizaremos del siguiente modo:

    ```
    sudo mkdir /var/www/jdlordinaria/t1/e1/info
    sudo mkdir /var/www/jdlordinaria/t1/e1/doc
    ```

    Modificamos el fichero de configuración de permisos, adaptandolo a los 2 directorios que creemos:

    ```
    <VirtualHost *:80>
        ServerName scrordinaria.t1.e1.es
        DocumentRoot /var/www/scrordinaria/t1/e1
    
        <Directory /var/www/scrordinaria/t1/e1>
            Options Indexes FollowSymLinks
            AllowOverride None
            Require all granted
        </Directory>
    
        Alias /info /var/www/scrordinaria/t1/e1/info/RGB/site/
        <Directory /var/www/scrordinaria/t1/e1/info/RGB/site/>
            Options Indexes FollowSymLinks
            AllowOverride None
            Require all granted
        </Directory>
    
        Alias /doc /var/www/scrordinaria/t1/e1/doc/docs/
        <Directory /var/www/scrordinaria/t1/e1/doc/docs/>
            Options Indexes FollowSymLinks
            AllowOverride None
            Require all granted
        </Directory>
    
        ErrorLog ${APACHE_LOG_DIR}/jdlordinaria.t1.e1.es_error.log
        CustomLog ${APACHE_LOG_DIR}/jdlordinaria.t1.e1.es_access.log combined
    </VirtualHost>
    ```
    
  * En info habrá un mkdoc con una página por cada color primario: RGB.

    Para ello, primero instalaremos mkdocs, en caso de no tenerlo:

    ```
    pip install mkdocs
    ```

    Nos dirigimos al fichero donde queremos generar nuestro MKDocs:

    ```
    cd /var/www/scrordinaria/t1/e1/info
    ```

    Accedemos al fichero y creamos el MKDocs:

    ```
    cd /var/www/jdlordinaria/t1/e1/info
    mkdocs new .
    ```
      – Página rojo, verde y azul. Más una página con enlace a ellas.
      
      Primero espeficicaremos nuestras paginas en el fichero mldoc.ylm:

      ```
      sudo nano mkdoc.yml
      ```

      Escribiremos lo siguiente:

      ```
      site_name: Mis Páginas de Color
      nav:
        - Inicio: index.md
        - Rojo: rojo.md
        - Verde: verde.md
        - Azul: azul.md
      ```

      A continuación, creamos los Markdown en nuestra carpeta docs desde por ejemplo nuestro IDE o directamente mediante la
      etiqueta ```sudo nano <nombre>.md```:

      Para los colores:

      ```md
      # Página Amarillo

      Esta es la página del color amarillo.
      ```

      Para el enlace entre las 3 paginas:
    
      ```md
      # Welcome to MkDocs

      For full documentation visit [mkdocs.org](https://www.mkdocs.org).
      
      ## Commands
      
      * `mkdocs new [dir-name]` - Create a new project.
      * `mkdocs serve` - Start the live-reloading docs server.
      * `mkdocs build` - Build the documentation site.
      * `mkdocs -h` - Print help message and exit.
      
      ## Project layout
      
          mkdocs.yml    # The configuration file.
          docs/
              index.md  # The documentation homepage.
              ...       # Other markdown pages, images and other files.
      
      # Enlaces a Colores Primarios
      
      Bienvenido a mi documentación. Aquí encontrarás información sobre colores primarios.
      
      - [Rojo](rojo.md)
      - [Verde](verde.md)
      - [Amarillo](amarillo.md)
      ```
      – Mete el mkdoc en la entrega final.
             
      Lo llevamos al fichero que tengamos para guardar nuestro proyecto

      – Haz un pantallazo de la página servida con mkdocs

      Para ello la servimos desde el directorio donde nos encontramos:
     
      ```
      sudo mkdocs serve 
      ```
      – Sube al servidor la página construida. CAPTURA

      La subimos con Firezilla, necesitaremos dar permisos al usuario desde el server:

      ```
      sudo chown -R scrordinaria:scrordinaria /scrordinaria
      ```
     
      Usamos el siguiente comando para subir nuestro proyecto al server:

      ```
      scp -r /var/www/sclordinaria/t1/e1/info/* sergio@scrordinaria.es:/var/www/sclordinaria/t1/e1/info
      ```
      – Haz un pantallazo de la página funcionando en el servidor.

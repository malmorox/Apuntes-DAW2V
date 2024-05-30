# Rewrite en Apache2

## Actimar el mod rewrite

```apache
sudo a2enmod rewrite
sudo systemctl restart apache2
```

### Ejemplo de Jorge

*index.php:*

```php
<?php
    $error = false

    if(isset($_GET['presentacion']) && isset($_GET['mensaje'])){
        $presentacion = $_GET['presentacion'];
        $mensaje = $_GET['mensaje'];
    } else {
        $error = true;
    }   
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Página PHP</title>
</head>
<body>
    <?php if (!$error): ?>
        <h1> <?= $presentacion; ?> </h1>
        <h2> <?= $mensaje; ?> </h1>
    <?php else: ?>
        <p> Ha habido un error </p>
    <?php endif; ?>
</body>
</html>
```

**Archivo de configuracion de Apache**

```apache
<VirtualHost *:80>
    ServerName paco.es

    ServerAdmin paco@localhost
    DocumentRoot /var/www/paco

    <Directory /var/www/paco>
        AllowOverride All
    </Directory>  

    ErrorLog ${APACHE_LOG_DIR}/paco.error.log
    CustomLog ${APACHE_LOG_DIR}/paco.access.log combined
</VirtualHost>
```

```apache
sudo a2ensite 004-paco.conf
sudo systemctl restart apache2
```

## Configurar las URLs Rewrite

**Archivo .htaccess**

```apache
RewriteEngine on
RewriteRule ^([^/]+)/([^/]+)/?$ index.php?presentacion=$1&mensaje=$2 [NC]
```

**Explicación de cada simbolo:**

- **^** indicata el comienzo de la URL, tras *ip_server/*

- **([^/]+)/([^/]+)** Expresión regular que coincide con cualquier secuencia de caracteres que no contenga una barra (/), seguida de otra que ahce lo mismo

- **/?** Indica que la barra final es opcional

- **$** indica el final del la URL

- **index.php?presentacion=$1&mensaje=$2** es el archivo al que el usuario accede, pasando los valores capturados como parámetros

- **[NC]** es una bandera que hace que la regla no distinga entre mayúsculas y minúsculas

## Navegador

*IMPORTANTE PON / AL FINAL* (si has no has puesto el **/?** al final de la regla del .htaccess)

> webname.dominio/xxx/xxx/

## Banderas de mod_rewrite

### <span style="color:cyan;">[L]</span> <span style="color:orange;">(Last)</span>

Detiene el procesamiento de reglas adicionales si esta regla coincide. Es como un "break" en un bucle.
Ejemplo: RewriteRule ^oldpage$ newpage <span style="color:green;">[L]</span>

### <span style="color:cyan;">[NC]</span> <span style="color:orange;">(NoCase)</span>

Hace que la regla no distinga entre mayúsculas y minúsculas.
Ejemplo: RewriteRule ^page$ page.html <span style="color:green;">[NC]</span>

### <span style="color:cyan;">[R]</span> <span style="color:orange;">(Redirect)</span>

Redirige el navegador a una URL externa. Por defecto, realiza una redirección 302 (temporal).
Puedes especificar un código de redirección opcional (301, 302, 303, 307, 308).
Ejemplo: RewriteRule ^oldpage$ <http://www.example.com/newpage> <span style="color:green;">[R=301,L]</span>

### <span style="color:cyan;">[QSA]</span> <span style="color:orange;">(Query String Append)</span>

Añade la cadena de consulta de la URL original a la nueva URL.
Ejemplo: RewriteRule ^page$ page.php <span style="color:green;">[QSA]</span> (Si la URL original era page?var=value, la nueva URL será page.php?var=value).

### <span style="color:cyan;">[QSD]</span> <span style="color:orange;">(Query String Discard)</span>

Descarta la cadena de consulta de la URL original.
Ejemplo: RewriteRule ^page$ page.php <span style="color:green;">[QSD]</span>

### <span style="color:cyan;">[NE]</span> <span style="color:orange;">(No Escape)</span>

No escapa caracteres especiales en la URL de destino.
Ejemplo: RewriteRule ^page$ page.php?param=value <span style="color:green">[NE]</span>

### <span style="color:cyan;">[PT]</span> <span style="color:orange;">(Pass Through)</span>

Pasa la URL reescrita al siguiente handler interno, útil cuando se combina con otras directivas como Alias.
Ejemplo: RewriteRule ^images/(.*)$ /var/www/images/$1 <span style="color:green">[PT]</span>

### <span style="color:cyan;">[S=X]</span> <span style="color:orange;">(Skip)</span>

Salta el procesamiento de las siguientes X reglas si la regla coincide.
Ejemplo: RewriteRule ^skipme$ - <span style="color:green;">[S=2]</span>

### <span style="color:cyan;">[E=VAR]</span> <span style="color:orange;">(Environment Variable)</span>

Establece una variable de entorno VAR con el valor VAL.
Ejemplo: RewriteRule ^page$ page.php <span style="color:green;">[E=VAR:value]</span>

### <span style="color:cyan;">[C]</span> <span style="color:orange;">(Chain)</span>

Encadena esta regla a la siguiente. Si esta regla coincide, la siguiente regla también será evaluada.
Ejemplo:
apache
Copiar código

```apache
RewriteRule ^foo$ bar [C]
RewriteRule ^bar$ baz
```

### <span style="color:cyan;">[T=MIME-type]</span> <span style="color:orange;">(Type)</span>

Establece el tipo MIME del contenido.
Ejemplo: RewriteRule ^image$ image.png <span style="color:green;">[T=image/png]</span>
Ejemplo de uso combinado
Aquí tienes un ejemplo que combina varias banderas:

```apache
RewriteEngine On
RewriteRule ^oldpage$ newpage [R=301,L]        # Redirección 301 y detener procesamiento
RewriteRule ^page$ page.php [QSA,NC]           # Añadir cadena de consulta y no distinguir mayúsculas
RewriteRule ^foo$ bar [PT]                     # Pasar a otro handler
RewriteRule ^baz$ baz.php [E=VAR:VALUE]        # Establecer variable de entorno
```

**Explicación de las banderas del ejemplo:**

- ***R=301,L:*** Redirige permanentemente (301) y detiene el procesamiento de reglas adicionales.
- ***QSA,NC:*** Añade la cadena de consulta a la nueva URL y hace que la regla no distinga entre mayúsculas y minúsculas.
- ***PT:*** Pasa la URL reescrita al siguiente handler interno.
- ***E=VAR:VALUE:*** Establece una variable de entorno VAR con el valor VALUE.

Estas banderas te permiten controlar el comportamiento de las reglas de reescritura de manera detallada y flexible.

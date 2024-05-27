# Rewrite en Apache2

## Actimar el mod rewrite

```
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
```
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

`webname.dominio/xxx/xxx/`

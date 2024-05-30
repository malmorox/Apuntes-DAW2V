# Servicios

## PHPDOCS

### Instalacion: [Documentación de instalación](https://docs.phpdoc.org/3.0/guide/getting-started/installing.html#installation)

```apache
 sudo apt install php8.1-cli
```

```apache
 sudo apt install php-mbstring
```

```apache
 mkdir myDocs
 cd myDocs
```

**En la carpeta personal (home):**

```apache
 wget https://phpdoc.org/phpDocumentor.phar
```

**Ejemplo de PHPDocs:**

```php
<?php
    class Perro {
        public $nombre;
        /**
        * Hace que el perro ladre
        *
        * @param integer $veces indica el número de ladridos
        * @param string $tipo indica el ladrido por defecto “Guau”
        *
        */
        public ladra(integer $veces, string $tipo="Guau")  {

        }
    }
?>
```

**Si queremos tener phpDocumentor en bin:**

```apache
 chmod a+x phpDocumentor.phar
```

```apache
$=> mv phpDocumentor.phar /usr/local/bin/phpDocumentor 
```

```apache
$=> phpDocumentor -d rutadelsrcconclases/ -t rutadondequeremoseldocs/docs/
```

**Si lo queremos mantener en el home:**

<span style="color:green;">// Hay que tener instalado el php-cli</span>

```apache
php ./phpDocumentor.phar
```

```apache
./phpDocumentor.phar run -d rutaDelSrc/ -t rutaDondeQueremosElDocs/docs/
```

**Si nos da error, deberemos instalar PHP XML, ya que puede ser que no venga instalado:**

```apache
$=> sudo apt install php8.1-xml
```

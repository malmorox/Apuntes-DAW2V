# PHPDOCS

### [Documentación de instalación](https://docs.phpdoc.org/3.0/guide/getting-started/installing.html#installation) (Página oficial)

#### Necesitaremos estos paquetes

```apache
sudo apt install php-cli php-mbstring php-xml
```

Creamos la carpeta que contendrán nuestros archivos PHP que queremos documentar:

```apache
mkdir myDocs
cd myDocs
```

**Ejemplo de clase para PHPDocs:** (Perro.php)

```php
<?php
    class Perro {
        public $nombre;
        /**
        * Hace que el perro ladre
        *
        * @param integer $veces indica el número de ladridos
        * @param string $tipo indica el ladrido por defecto “Guau”
        * @return string Los ladridos del perro
        *
        */
        public ladra(integer $veces, string $tipo="Guau")  {
            $ladridos = "";
            for ($i = 0; $i < $veces; $i++) {
                $ladridos .= $tipo . " ";
            }
            return $ladridos;
        }
    }
?>
```

## Instalación de PHPDocumentor

### En la carpeta personal (home)

```apache
wget https://phpdoc.org/phpDocumentor.phar
```

```apache
chmod a+x phpDocumentor.phar
```

**Si queremos tener phpDocumentor en bin:** (Como comando)

```apache
mv phpDocumentor.phar /usr/local/bin/phpDocumentor 
```

Al runnearlo, deberemos indicarle la carpeta donde estan nuestros archivos PHP para la documentación (en este caso la ruta de **myDocs**) y la carpeta tarjet donde se va a desplegar nuestro PHPDocs

```apache
phpDocumentor -d rutaDelSrc/ -t rutaDondeQueremosElDocs/docs/
```

**Si lo queremos mantener en el home:** (y buildearlo con PHP)

```apache
php ./phpDocumentor.phar
```

```apache
./phpDocumentor.phar run -d rutaDelSrc/ -t rutaDondeQueremosElDocs/docs/
```

Después de haberlo desplegado tendremos que pasarle al servidor, por FileZilla, la carpeta de docs con todas su contenido.

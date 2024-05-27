En las reglas de reescritura de Apache (mod_rewrite), se pueden usar varias banderas para modificar el comportamiento de las reglas. Las banderas se especifican entre corchetes al final de una regla de reescritura. Aquí te doy una lista de las banderas más comunes y su descripción:

## Banderas de mod_rewrite

### [L] (Last)

Detiene el procesamiento de reglas adicionales si esta regla coincide. Es como un "break" en un bucle.
Ejemplo: RewriteRule ^oldpage$ newpage [L]

### [NC] (NoCase)

Hace que la regla no distinga entre mayúsculas y minúsculas.
Ejemplo: RewriteRule ^page$ page.html [NC]

### [R] (Redirect)

Redirige el navegador a una URL externa. Por defecto, realiza una redirección 302 (temporal).
Puedes especificar un código de redirección opcional (301, 302, 303, 307, 308).
Ejemplo: RewriteRule ^oldpage$ http://www.example.com/newpage [R=301,L]

### [QSA] (Query String Append)

Añade la cadena de consulta de la URL original a la nueva URL.
Ejemplo: RewriteRule ^page$ page.php [QSA] (Si la URL original era page?var=value, la nueva URL será page.php?var=value).

### [QSD] (Query String Discard)

Descarta la cadena de consulta de la URL original.
Ejemplo: RewriteRule ^page$ page.php [QSD]

### [NE] (No Escape)

No escapa caracteres especiales en la URL de destino.
Ejemplo: RewriteRule ^page$ page.php?param=value [NE]

### [PT] (Pass Through)

Pasa la URL reescrita al siguiente handler interno, útil cuando se combina con otras directivas como Alias.
Ejemplo: RewriteRule ^images/(.*)$ /var/www/images/$1 [PT]

### [S=X] (Skip)

Salta el procesamiento de las siguientes X reglas si la regla coincide.
Ejemplo: RewriteRule ^skipme$ - [S=2]

### [E=VAR] (Environment Variable)

Establece una variable de entorno VAR con el valor VAL.
Ejemplo: RewriteRule ^page$ page.php [E=VAR:value]

### [C] (Chain)

Encadena esta regla a la siguiente. Si esta regla coincide, la siguiente regla también será evaluada.
Ejemplo:
apache
Copiar código
```apache
RewriteRule ^foo$ bar [C]
RewriteRule ^bar$ baz
```

### [T=MIME-type] (Type)

Establece el tipo MIME del contenido.
Ejemplo: RewriteRule ^image$ image.png [T=image/png]
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
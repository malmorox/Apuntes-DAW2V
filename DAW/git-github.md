# Git y Github

## ¿Qué necesitamos?

Necesitaremos git:

```apache
$=> sudo apt install git
```

## Repositorio en local con Git

### Crear el repositorio git

Comenzamos creando el directorio donde quereamos inicicalizar el repositorio git:

```apache
$=> mkdir repositorio
$=> cd repositorio/
```

Inicializamos git:

```apache
$=> git init
```

### Borrar repositorio git

```apache
$=> rm -r .git/
```

### Introducir datos usuario al git local

```apache
$=> git config--global user.name nombre
$=> git config--global user.mail nombre@daw.git
```

### Comandos para guardar cambios

```apache
$=> git status // Estado del repositorio
```

```apache
$=> git add // Añadir al Stage
```

```apache
$=> git commit // Hecha la foto
```
# Git y Github

## ¿Qué necesitamos?

Necesitaremos git:

```bash
 sudo apt install git
```

## Repositorio en local con Git

### Crear el repositorio git

Comenzamos creando el directorio donde quereamos inicicalizar el repositorio git:

```bash
 mkdir repositorio
 cd repositorio/
```

Inicializamos git:

```bash
 git init
```

### Borrar repositorio git

```bash
 rm -r .git/
```

### Introducir datos usuario al git local

```bash
 git config--global user.name nombre
 git config--global user.mail nombre@daw.git
```

### Comandos para guardar cambios

```bash
 git status // Estado del repositorio
```

```bash
 git add // Añadir al Stage
```

```bash
 git commit // Hecha la foto
```

### Comandos historial, cambio de ramas y merge

```bash
* git log --all --gr> //Historial de los commi
* git diff // Para ver las diferencias entre el archivo del repositorio y el que yo estoy usando
* git checkout commitCode //Ir al commit que le hemos dicho
* git switch -c nuevaRama //Crea y cambia de rama a la creada
* git merge nombreRama //Junta la rama en la que estamos con la rama nombreRama
```

### Claves ssh

```bash
* ssh-keygen //se crea por defecto en la ruta ~/.ssh/id_rsa
* cp ~/.ssh/id_rsa.pub //copiar la clave publica
```

### Crear el repositorio local al que hacer push y pull

```bash
* git init --bare nombreRepositorio.git
* git clone /rutaRepo/nombreRepositorio.git //Trabajar como con GitHub/GitLab
```
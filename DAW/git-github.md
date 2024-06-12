# GIT Y GITHUB

## ¿Qué necesitamos?

Necesitaremos git:

```apache
 sudo apt install git
```

## Manejo de repositorios

### Creación de un repositorio bare

#### Crear el directorio

```bash
mkdir /home/usuario/repositorio
```

#### Ir al directorio

```bash
cd /home/usuario/repositorio
```

#### Inicializar el repositorio bare

```
git init --bare mi-repositorio.git
```

### Creación de un repositorio

#### Crear el directorio

```
mkdir /home/usuario/repositorio
```

#### Ir al directorio

```
cd /home/usuario/repositorio
```

#### Inicializar el repositorio

```
git init
```

### Borrar un repositorio bare

```
cd /home/usuario
rm -rf /repositorio
```

### Borrar un repositorio

```
cd /home/usuario/repositorio
rm -rf .git/
```

### Clonación de un repositorio bare

```
git clone /home/usuario/repositorio
```

### Clonación de un repositorio

#### Con SSH

```
git clone usuario@servidor:/home/usuario/repositorio.git
```

#### Con HTTPS

```
git clone https://servidor/repositorio.git
```

## Uso de repositorios

### Ver estado

```
git status
```

### Añadir archivos al repositorio

```
git add .
```

### Crear commit

```
git commit -m "Mensaje"
```

### Subir archivos al repositorio

```
git push
```

### Actualizar repositorio

```
git pull
```

### Crear rama

#### Crearla

```
git branch rama
```

#### Cambiarse a ella

```
git checkout rama
```

#### Crearla y cambiarse a ella

```
git checkout -b rama
```

### Fusionar ramas

#### Primero ir a la rama a la que le queremos fusionar la otra

```
git checkout main
```

#### Una vez allí, fusionar la rama

```
git merge rama
```

### Ver ramas

```
git branch
```

### Eliminar rama

```
git branch -d rama
```

### Ver historial

```
git log
```

#### Historial de un archivo

```
git log archivo
```

#### Historial de una rama

```
git log rama
```

#### Historial gráfico

```
git log --graph
```

#### Todo lo anterior en una línea

```
git log --all --graph --oneline
```

### Ver diferencias

```
git diff
```

### Ver diferencias entre ramas

```
git diff rama1 rama2
```

### Ver diferencias entre commits

```
git diff commit1 commit2
```

### Ignorar archivos

```
nano .gitignore
```

### Amend

#### Añadir archivos al último commit

```
git commit -m 'Commit message'
git add archivo_olvidado
git commit --amend
```

#### Cambiar el mensaje del último commit

```
git commit -m 'Commit message'
git add archivo_olvidado
git commit --amend -m 'Nuevo mensaje'
```

### Reset

#### Resetear el último commit

```
git reset --soft HEAD~1
```

#### Resetear el último commit y quitar los archivos del staging area

```
git reset --mixed HEAD~1
```

#### Resetear el último commit y quitar los archivos del staging area y del working directory

```
git reset --hard HEAD~1
```

#### Resetear el último commit, quitar los archivos del staging area y del working directory y aplicar al ancestro primero

```
git reset --hard HEAD^
```

### Revert

#### Revertir el último commit

```
git revert HEAD
```

#### Revertir un commit concreto

```
git revert numero_commit
```

### Stash

#### Guardar cambios en el stash

```
git stash
```

#### Ver los cambios guardados en el stash

```
git stash list
```

#### Aplicar los cambios guardados en el stash

```
git stash apply
```

#### Eliminar los cambios guardados en el stash

```
git stash drop
```

#### Aplicar y eliminar los cambios guardados en el stash

```
git stash pop
```

Y añadimos los archivos que queremos ignorar, por ejemplo:

```
*.log
/tmp
*.idea
```
# Cron y Crontab

## Crear un fichero

```
touch tarea.sh
chmod ugo+x consulta.sh
vim tarea.sh
```

**Ejemplo de sript para hacer una copia de seguridad**

```bash
#!/bin/bash
tar -czvf backup`date+%d%b%y`.tar /ruta/directorioWeb
```

<img src="./img/crontab.webp">

**Ejemplo linea de `crontab -e` son 5 * y la ruta**

```bash
minuto mes diaMes mes diaSemana /ruta/archivo.sh
```

## Administrar Trabajos del Cron

**Remplazar archivo existente por uno que defina el usuario**

`crontab archivo`

**Editar archivo existente de crontab**

`crontab -e`

**Listar todas las tareas existentes en el crontab del usuario**

`crontab -l`

**Borrar el crontab que está configurado**

`crontab -d`

**Definir donde se almacena el archivo de corntab**

`crontab -c dir`

**Borrar el archivo de crontab de manera permanente**

`crontab -r`
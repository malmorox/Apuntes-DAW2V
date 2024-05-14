## Comando de base de datos para terminal

1. Crear base de datos:

        CREATE DATABASE nombre_de_la_base_de_datos;

2. Crear usuario:

        CREATE USER 'nombre_usuario'@'localhost' IDENTIFIED BY 'contraseña';

        CREATE USER 'TWITTER'@'localhost' IDENTIFIED BY 'TWITTER';
3. Otorgar permisos al usuario para la base de datos:

        GRANT ALL PRIVILEGES ON nombre_de_la_base_de_datos.* TO 'nombre_usuario'@'localhost';

        FLUSH PRIVILEGES;

    **Nota:**
            Siempre ejecutar *FLUSH PRIVILEGES*; después de hacer cambios significativos en  los permisos para asegurar que los cambios tomen efecto inmediatamente.

4. Comando para conectarse desde terminal:

        mysql -u nombre_usuario -p contraseña_bbdd -D nombre_de_la_base_de_datos
                    -u : usuario
                    -p : contraseña de la bbdd
                    -D : base de datos

5. si queremos conectarnos a la base de datos para redireccionar el archivo con las tablas:

        mysql -u nombre_usuario -p contraseña_bbdd < nombre_de_la_base_de_datos.sql


6. Comando para desconectarse:

        QUIT;


7. Comando para salir de la terminal:


        EXIT;



### Comandos adicionales:
1. Ver las distintas base de datos:
   
        SHOW DATABASES;

2. Seleccionar una base de datos para usar:
   
        USE nombre_de_la_base_de_datos;

3. Mostrar tablas en la base de datos actual:
   
        SHOW TABLES;

4. Mostrar la estructura de una tabla:
   
        DESCRIBE nombre_de_la_tabla;
        o
        DESC nombre_de_la_tabla;

5. Eliminar la base de datos:
   
        DROP DATABASE nombre_de_la_base_de_datos;

6. Eliminar la tabla:
   
        DROP TABLE nombre_de_la_tabla;

7. Eliminar el usuario:
   
        DROP USER 'nombre_usuario'@'localhost';

8. Ver los usuarios existentes:
   
        SELECT user, host FROM mysql.user;

9.  Ver los permisos de un usuario:
    
        SHOW GRANTS FOR 'nombre_usuario'@'localhost';
        o
        SELECT * FROM mysql.user WHERE user = 'nombre_usuario';

10. Eliminar los permisos de un usuario:
    
        REVOKE ALL PRIVILEGES ON nombre_de_la_base_de_datos.* FROM 'nombre_usuario'@'localhost';

        FLUSH PRIVILEGES;

----------RESPALDO BACKUP------------------------

11. Para hacer un respaldo de la base de datos:

        mysqldump > db_bseDe_datos.sql

12. Para restaurar la base de datos:
    
        mysql -u acciones -p acciones < db_baseDe_datos.sql


13. Para asegurarnos que estamos usando la base de datos correcta:
        
        SELECT database();



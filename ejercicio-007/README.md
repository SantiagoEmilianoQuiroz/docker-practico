# Ejercicio 7

Se necesita asegurar la persostencia de una base de datos MySQL ejecutada en Docker, de manera que la informacion no se pierda al detener o eliminar el contenedor. Para resolverlo, crea un volumen para almacenar los datos de MySQL, ejecuta un contenedor con la imagen oficial de MySQL montando ese volumen y definiendo la contraseña de usuario root mediante una variable de entorno, y luego verifica que al detener y eliminar el contenedor, al recrearlo los datos, tablas y registros sigan intactos . El resultado esperado es que las bases de datos permanezcan accesibles y consistentes mientras el volumen exista, garantizando la permanencia de la informacion.

### Paso 1: Creamos la imagen y corremos el contenedor
```
docker run -d  -name mysql-con-volumen -p 3307:3306 -e MYSQL_ROOT_PASSWORD=password -v mis-datos-db:/var/lib/mysql mysql

```
- **--name mysql-con-volumen**: asigna un nombre fácil de recordar al contenedor (mysql-con-volumen). Esto te permite detenerlo o reiniciarlo sin tener que buscar su código de identificación.
- **-p 3307:3306**:conecta los puertos entre el host y el contenedor. El primer número (3307) es el puerto en el equipo local, y el segundo (3306) es el puerto interno donde escucha MySQL.
- **-e MYSQL_ROOT_PASSWORD=password**: define una variable de entorno obligatoria para la imagen de MySQL. Configura la contraseña del usuario administrador (root) como password.
- **-v mis-datos-db:/var/lib/mysql**: crea un volumen persistente llamado mis-datos-db y lo vincula a la carpeta /var/lib/mysql dentro del contenedor. Gracias a esto, tus tablas y registros no se borrarán si eliminas el contenedor.
- **Mysql**: es el nombre de la imagen oficial que Docker descargará de Docker Hub (en su versión más reciente) para construir el contenedor.

### Paso 2: Ingresamos al contenedor para modificar datos

```
docker exec -it mysql-con-volumen mysql -u root -p

```

- **-u root**: indica que quieres iniciar sesión usando el usuario administrador llamado root.
- **-p**: le pide a MySQL que solicite la contraseña antes de dejarte entrar. Al presionar Enter, la terminal te pedirá escribir password (los caracteres no se verán en pantalla por seguridad).

### Paso 3: Creamos datos en la base de datos MySQL

```
-- 1. Crear la base de datos
CREATE DATABASE prueba_persistencia;

-- 2. Seleccionar la base de datos
USE prueba_persistencia;

-- 3. Crear una tabla simple
CREATE TABLE usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL
);

-- 4. Insertar un registro
INSERT INTO usuarios (nombre) VALUES ('Santi');

-- 5. Verificar que el dato existe
SELECT * FROM usuarios;

-- 6. Salir de la terminal de MySQL
EXIT;

```

### Paso 4: Eliminamos el contenedor y volvemos  a crear otro, luego verificamos que la la informacion de la base de datos persista en el volumen

```
#Eliminamos el contenedor
docker rm -f mysql-con-volumen

#Creamos un contenedor nuevo
docker run -d -it -- name mysql-nuevo -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password -v mis-datos-db:/var/lib/mysql mysql

#Ingresamos a la base de datos
docker exec -it mysql-nuevo mysql -u root -p

#Verificamos que la informacion siga en el volumen
USE prueba_persistencia;
SELECT * FROM usuarios;

```

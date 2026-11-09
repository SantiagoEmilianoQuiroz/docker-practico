# Ejercicio 2

Se necesita aprender a usar contenedores Docker de manera interactica para ejecutar comandos basicos en un entorno Linux sin afectar el sistema host. Para ello. inicia un contenedor Ubuntu 20.04 en modo interactivo, actualiza la lista de paquetes, instala un editor de texto, crea un archivo llamado hola.txt con contenido dentro del contenedor, verifica su contenido y finalmente cierra y elimina el contenedor. El resultado esperado es que todos los comandos se ejecuten conrrectamente dentro del contenedor, el archivo se cree con exito y el contenedor se elimine sin dejar residuos en el host.

### Paso 1: Primer comando

```
docker run -it --name my-ubuntu ubuntu:20.04 bash

```

- **-it**: conbina dos banderas:
    - **-i**: mantiene abierta la salida estandar (STDIN) del contenedor, lo que permite escribir comandos y enviarselos
    - **-t**: asigna una terminal virtual (TTY). Hace que se vea el prompt clasico de linux.

- **bash**: es el comando de inicio que se ejecutara dentro del contenedor tan pronto como este arranque. Al especificar bash, le estas pidiendo a Docker que reemplace el comando por defecto de la imagen y te deje dentro del interprete de comandos Bash de Ubuntu.

### Paso 2: Actualizamos los paquetes del servidor

```
apt update

```
### Paso 3: Creamos el archivo indicado

```
echo "Hola desde Docker" > hola.sh

```

### Paso 4: Instalamos un editor de texto

```
apt install nano

```

### Paso 5: Salimos y eliminamos el contenedor y la imagen

```
exit

docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>
docker rmi <IMAGE_ID>

```


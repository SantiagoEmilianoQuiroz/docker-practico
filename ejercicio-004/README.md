# Ejercicio 4

Se necesita despelegar una aplicacion web Flask dentro de un contenedor Docker, accesible desde el navegador por el puerto 5000, incluyendo todas sus dependencias sin afectar al host. Para ello, crea un contenedor usando una imagen base de Python 3.9 slim, configura el directorio de trabajo, copia e instala las dependencias instaladas correspondientes y define el comando de ejecucion. El resultado esperado es que al acceder desde el navegador a la direccion del host y el puerto asignado, se muestre el mensaje "Hola desde el contenedor Docker!!!" y que el contenedor pueda ejecutarse, detenerse y eliminarse correctamente.

### Paso 1: Crear el Dockerfile

```

FROM python:3.9-slim -> version de python

WORKDIR /app -> directorio de trabajo

COPY requirements.txt . -> copiamos los requisitos

RUN pip install -r requirements.txt -> comandos a correr en la etapa de creacion de la imagen


COPY . . -> copiamos el proyecto dentro del contenedor

EXPOSE 5000 -> exponemos el puerto 5000

CMD ["flask", "run", "--host=0.0.0.0", "--port=5000"] -> comandos a ejecutar cuando inicie el contenedor

```
- **"--host=0.0.0.0"**: permite que la aplicacion sea accesible fuera del contenedor

### Paso 2: Hacemos el build del Dockerfile
```

docker build -t app-python .

```
### Paso 3: Corremos el contenedor

```
docker run -d -p 5000:5000 app-python:latest 

```

- **-p 5000:5000**: mapea el puerto local al puerto del contenedor
# Ejercicio 5

Se necesita deplegar una aplicacion web FastAPI dentro de un contenedor Docker, accesible desde el navegador o mediante clientes HTTP en el puerto 8000, incluyendo imagen basePython 3.9 slim, configura el directorio de trabajo, copia e instala las dependencias instaladas en el requirements.txt, agrega el codigo de la aplicacion FastAPI, expon el puerto correspondiente y define el comando de ejecucion con Uvicom. el resultado esperado es que al acceder a la ruta raiz de la aplicaion se reciba la respuesta {"message":"Hola mundo} y que el contenedor pueda iniciarse, detenerse y eliminarse correctamente

### Paso 1: Crear el Dockerfile

```
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install  -r requirements.txt

COPY app.py .

EXPOSE 8000

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]

```
### Paso 2: Hacemos el build de la imagen

```
docker build -t app-python .

```

### Paso 3: Corremos el contenedor de la imagen

```
docker run -d -p 8000:8000 app-python:latest 

```

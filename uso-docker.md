# Comandos básicos de Docker

## Descarga y configuración de una imagen existente

El siguiene comando es un ejemplo real de como se descarga una imagen y se crea un contenedor con una configuración optima para el uso de inteligencia artificial.

```docker run -it --gpus all -p 9999:8888 -v $PWD:/workspace --name torchmd-net-tesis --shm-size 16G emmanuelzula/tesis:0.1 /bin/bash```

El comando tiene las siguiente instrucciones:

1. **docker run**: Este es el comando principal de Docker para ejecutar un contenedor.

2. **-it**: Este es el argumento que se utiliza para indicar que se desea una terminal interactiva. Esto permite interactuar con el contenedor a través de la línea de comandos.

3. **--gpus all**: Indica que se asignaran todos los recursos de GPU disponibles en al contenedor. Esto asume que tienes GPU y que se ha configurado Docker para admitir GPU.

4. **-p 9999:8888**: Esto mapea el puerto 8888 del contenedor al puerto 9999 de tu host local. Significa que si el contenedor ejecuta un servicio en el puerto 8888, se puede acceder a él desde tu navegador en `localhost:9999`.

5. **-v $PWD:/workspace**: Este argumento establece un volumen (mount) que vincula el directorio actual (`$PWD`) en el host local con el directorio `/workspace` en el contenedor. Esto permite compartir archivos y datos entre el sistema local y el contenedor.

6. **--name torchmd-net**: Asigna un nombre al contenedor, en este caso, "torchmd-net". Se puede usar este nombre para hacer referencia al contenedor en lugar de su identificador largo.

7. **--shm-size 16G**: Esto configura el tamaño de la memoria compartida (shared memory) dentro del contenedor en 16 gigabytes.

8. **emmanuelzula/servicio:1.3**: Es la imagen de Docker que se utilizará para crear el contenedor. En este caso, se está utilizando la imagen "emmanuelzula/servicio" con la etiqueta "1.3".

9. **/bin/bash**: Es el comando que se ejecutará dentro del contenedor. En este caso, se inicia un shell Bash dentro del contenedor, lo que te permite interactuar con él.

Una vez descargado el contenedor nos encontraremos dentro de el mediante una terminal podemos salir de el sin ningun problema.

## Uso de un contenedor existente

Para hacer uso de un contenedor primero debemos inciarlo con el comando ```docker start "nombre-del-contenedor"``` siguiendo el ejemplo anterior el comdando seria.

```bash
docker start torchmd-net
```

Ya esta iniciado nuestro contenedor, ahora hay que ejecutar un comando dentro de él. Para eso ejecutamos el comando ```docker exec``` para ejecutar el programa ```/bin/bash``` en modo iterativo ```-it``` dentro del contenedor de nombre ```torchmd-net```

```bash
docker exec -it torchmd-net /bin/bash
```

Ya estamos en una terminal ejecutandose dentro del contenedor, recordemos salir de contenedor con el comando ```exit``` y detenerlo con el comando ```docker stop torchmd-net```

## Guardar un contenedor como imagen docker

Podemos descargar cualquier imagen y crear contenedores a partir de ella, modificar los contenedores a nuestra conveniencia (instalar codigo de machine learnign por ejemplo), para después guardar el contenedor como una imagen que no se verá afectada por cambios al contenedor y de la cual se podran crear nuevos contenedores. Eso lo podemos hacer con el comando ```docker commit CONTAINER ID Usuario/imagen:tag```

Siguiendo el ejemplo, el comando seria:

```docker commit torchmd-net emmanuelzula/servicio:1.4```

De esta forma la imagen quedará guardada de forma local.

## Subir imagen docker a docker hub

Creamos una cuenta en docker hub, para acceder al ella en la terminal (fuera del contenedor) introducimos el comando ```docker login``` introducimos nuestras credenciales y ya podemos acceder a nuestra cuenta de docker hub.

Para subir nuestra imagen introducimos el comando ```docker push Usuario/imagen:tag``` En este caso, siguiendo el ejemplo.

```docker push emmanuelzula/servicio:1.4```

Recordemos salir de nuestra cuenta de docker hub con el comando ```docker logout```

## Configuración avanzada (DockerFile)
Podemos añadir instrucciones y funcionalidades más especificas a la hora de crear una imagen, para eso se cuenta con el archivo DockerFile (sin extención de archivo), un ejemplo del tipo de instrucciones que podemos añadir se encuentra abajo.

```DockerFile
# Usa una imagen base de emmanuelzula
FROM emmanuelzula/servivio:1.4

# Documenta la intención de exponer el puerto 8888, pero no lo publica por sí mismo
# Recuerda usar -p al ejecutar el contenedor para mapear el puerto de host al contenedor
EXPOSE 8888/tcp

# Establece el directorio de trabajo en /workspace
WORKDIR /workspace

# Ejecuta un comando durante la construcción de la imagen
# Agrega una línea al final de ~/.bashrc para activar el ambiente virtual 'torchmd-net'
RUN /bin/sh -c 'echo "mamba activate torchmd-net" >> ~/.bashrc'

# Configura la apertura de la terminal en el directorio de trabajo
RUN echo "cd /workspace" >> ~/.bashrc
```

Construimos la imagen con la configuración del docker file con el siguiente comando ```docker build -t Usuario/imagen:tag .```

En este caso

```bash
docker build -t emmanuelzula/servivio:1.5 .
```
El comando se debe ejecutar en el mismo directorio del dockerfile

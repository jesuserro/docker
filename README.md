# Docker Hello World

Ejemplo uso docker
Crea una imagen para poder ejecutar un proyecto php-apache.

## Conceptos básicos

Diferenciar conceptos `imagen` (clase) y `contenedor` (objeto)
El flujo de creación es el siguiente: 1 yml file -> 1 Dockerfile -> 1 Imagen -> 1 Contenedor.

El Dockerfile define las instrucciones para crear una imagen.

La imagen se genera a partir de un Dockerfile con el comando `build`. La imagen contiene 3 cosas: sistema operativo (Ubuntu, Windows), software (LAMP) y tu app (código)

Para crear un contenedor hay que correr la imagen con la instrucción `run`. Esto hace la descarga de la imagen a tu dispositivo.

## Forma simple

### Para crear las imagenes

``` shell
docker build -t php_apache:7.4 -f Dockerfile.74 .
docker build -t php_apache:8.2 -f Dockerfile.82 .
```

### Para lanzar los contenedores

``` shell
docker run -d -p 8082:80 -v /home/username/proyectos:/var/www/html --name php82 php_apache:8.2
docker run -d -p 8074:80 -v /home/username/proyectos:/var/www/html --name php74 php_apache:7.4
```

### Para probarlo en el navegador

``` shell
localhost:8074
localhost:8082
```

## Usando Docker Compose

``` shell
cd /home/username/proyectos/docker

# remove all docker containers  
docker container rm -f $(docker container ls -aq)
# command to remove all docker images  
docker rmi $(docker images -q)

docker compose up

docker exec -it php82-apache /bin/bash

docker logs php82-apache
```

## Tutoriales

<iframe width="560" height="315" src="https://www.youtube.com/embed/CV_Uf3Dq-EU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/6idFknRIOp4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

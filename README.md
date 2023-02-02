# Docker Hello World

Ejemplo uso docker. Creación de 2 containers apache con php 7.4 y php 8.2.
Se hace a partir de la imagen oficial de PHP en Docker Hub: <https://hub.docker.com/_/php>

## Instalacion

``` shell
mkdir ~/proyectos && cd ~/proyectos
git clone https://github.com/jesuserro/docker.git
```

## Levantando containers simples

``` shell
# Para crear las imagenes
docker build -t php_apache:7.4 -f Dockerfile.74 .
docker build -t php_apache:8.2 -f Dockerfile.82 .

# Para lanzar los contenedores
docker run -d -p 8082:80 -v /home/username/proyectos:/var/www/html --name php82 php_apache:8.2
docker run -d -p 8074:80 -v /home/username/proyectos:/var/www/html --name php74 php_apache:7.4

# Para probarlo en tu navegador
localhost:8074
localhost:8082
```

## Levantando containers con Docker Compose

``` shell
cd /home/username/proyectos/docker

# Levantando contenedores definidos en docker-compose.yml
docker compose up

# En tu navegador
localhost:8082

# Abriendo terminal del contenedor
docker exec -it php82-apache /bin/bash

# Mostrando logs del contenedor
docker logs php82-apache

# Parando contenedores
docker stop php82-apache

# Removing all previous docker containers  
docker container rm -f $(docker container ls -aq)

# Removing all previous docker images  
docker rmi $(docker images -q)
```

## Tutoriales

### DOCKER De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL)

[![DOCKER De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL)](https://img.youtube.com/vi/CV_Uf3Dq-EU/0.jpg)](https://www.youtube.com/watch?v=CV_Uf3Dq-EU)

### Aprende Docker en 14 minutos 🐳

[![Aprende Docker en 14 minutos 🐳](https://img.youtube.com/vi/6idFknRIOp4/0.jpg)](https://www.youtube.com/watch?v=6idFknRIOp4)

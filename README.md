# Docker Hello World

Simple example of using Docker. Creation of 2 Apache containers with PHP 7.0 to PHP 8.2.
It is made from the official PHP image on Docker Hub: <https://hub.docker.com/_/php>

These Dockerfiles, specially Dockerfile 7.0, install the required packages and PHP extensions, set the PATH environment variable, install Composer, change the current working directory, and set the owner of the container document root. They then install XDEBUG, enable rewrite mode, and set the Apache document root and virtual host parameters using the techniques described earlier. Finally, they starts Apache in foreground mode.

## Instalación

``` shell
mkdir ~/proyectos && cd ~/proyectos
git clone https://github.com/jesuserro/docker.git
```

## Levantando containers simples

``` shell
# Levantar container con yaml
docker compose up

# Para reconstruir imagenes ya creadas
docker build -t php_apache:7.0 -f Dockerfile.70 .
docker build -t php_apache:7.4 -f Dockerfile.74 .
docker build -t php_apache:8.2 -f Dockerfile.82 .

# Para lanzar los contenedores
docker run -d -p 8082:80 -v /home/jesus/proyectos:/var/www/html --name php82 php_apache:8.2
docker run -d -p 8074:80 -v /home/jesus/proyectos:/var/www/html --name php74 php_apache:7.4

# Para probarlo en tu navegador
localhost:8074
localhost:8082
```

## Levantando containers con Docker Compose

``` shell
cd /home/jesus/proyectos/docker

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

# Removing all docker containers  
docker container rm -f $(docker container ls -aq)

# Removing all docker images  
docker rmi $(docker images -q)
```

## VSCODE

### XDEBUG

In the `launch.json` in your VSCODE project, add this:

``` shell
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Docker Xdebug",
      "type": "php",
      "request": "launch",
      "port": 9000,
      "pathMappings": {
          "/var/www/html/YOUR_PROJECT": "${workspaceFolder}"
      },
      "hostname": "localhost",
      "log": true,
      "xdebugSettings": {
          "max_data": -1
      }
    }    
  ]
}
```

### Php Executable

``` shell
# Getting executable from the container
docker cp php70-apache:/usr/local/bin/php ./var/bin/php
# In VSCODE settings.json
"php.executablePath": "./var/bin/php"

# Executable from your local LAMP
which php
# In VSCODE settings.json
"php.executablePath": "/usr/bin/php"
```

## Tutoriales

### DOCKER De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL)

[![DOCKER De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL)](https://img.youtube.com/vi/CV_Uf3Dq-EU/0.jpg)](https://www.youtube.com/watch?v=CV_Uf3Dq-EU)

### Aprende Docker en 14 minutos 🐳

[![Aprende Docker en 14 minutos 🐳](https://img.youtube.com/vi/6idFknRIOp4/0.jpg)](https://www.youtube.com/watch?v=6idFknRIOp4)

## Otros enlaces de interés

### De PHP 5.3 a PHP 8.2

[![De PHP 5.3 a PHP 8.2](https://img.youtube.com/vi/BHAYO6esXlw/0.jpg)](https://www.youtube.com/watch?v=BHAYO6esXlw)

- [Migración de Zend Framework 1 a PHP 8.1](https://github.com/Shardj/zf1-future)

- [Terraform](https://registry.terraform.io/) a cloud management service for AWS, Google Cloud, Azure, and so on.

## Interesting Dockerfiles

- <https://jasonterando.medium.com/debugging-with-visual-studio-code-xdebug-and-docker-on-windows-b63a10b0dec>
- <https://github.com/thecodeholic/php-mvc-framework>
- <https://stackoverflow.com/questions/52579102/debug-php-with-vscode-and-docker>

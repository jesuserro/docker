# Docker Hello World.

Ejemplo uso docker
Crea una imagen para poder ejecutar un proyecto php-apache

## Para crear las imagenes
docker build -t php_apache:7.4 -f Dockerfile.74 .
docker build -t php_apache:8.0 -f Dockerfile.80 .
docker build -t php_apache:8.1 -f Dockerfile.81 .
docker build -t php_apache:8.2 -f Dockerfile.82 .

## Para lanzar los contenedores
docker run -d -p 8082:80 -v /mnt/c/proyectos:/var/www/html --name php82 php_apache:8.2
docker run -d -p 8081:80 -v /mnt/c/proyectos:/var/www/html --name php81 php_apache:8.1
docker run -d -p 8080:80 -v /mnt/c/proyectos:/var/www/html --name php80 php_apache:8.0
docker run -d -p 8074:80 -v /mnt/c/proyectos:/var/www/html --name php74 php_apache:7.4

## Para probarlo en el navegador
localhost:8074
localhost:8080
localhost:8081
localhost:8082

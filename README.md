# My Docker ecosystem

Simple example of using Docker. Creation of containers with PHP 7/8 and XDebug.
It is made from the official PHP image on Docker Hub: <https://hub.docker.com/_/php>

These Dockerfiles, specially Dockerfile 7.0, install the required packages and PHP extensions, set the PATH environment variable, install Composer, change the current working directory, and set the owner of the container document root. They then install XDEBUG, enable rewrite mode, and set the Apache document root and virtual host parameters using the techniques described earlier. Finally, they starts NGINX/Apache in foreground mode.

## Instalación

``` shell
mkdir ~/proyectos && cd ~/proyectos
git clone https://github.com/jesuserro/docker.git
```

## Levantando containers simples

``` shell
cd ~/proyectos/docker

# Levantar container con yaml
docker compose up

# Para reconstruir imagenes ya creadas
docker build -t php_apache:7.0 -f Dockerfile.70 .
docker build -t php_apache:8.2 -f Dockerfile.82 .

# Para lanzar los contenedores
docker run -d -p 8070:80 -v /home/jesus/proyectos:/var/www/html --name php74 php_apache:7.0
docker run -d -p 8082:80 -v /home/jesus/proyectos:/var/www/html --name php82 php_apache:8.2

# In your browser
http://localhost:8070
http://localhost:8082
http://localhost:8070/ofertas/public/admin/index.php
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

# Removing all docker volumes and launching compose.yml in 1 sentence
docker container rm -f $(docker container ls -aq) && docker rmi $(docker images -q) && clear && docker compose up
```

## VSCODE

### XDebug

In your WSL2 terminal, run this command to get your IP address:

``` shell
ip addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'
```

So put it at the xdebug.ini file:

``` shell
xdebug.remote_host = 172.20.28.159
```

If the IP is correct, you should see the Call Stack requests in the Debug Pane at VSCODE.
Now, in the `launch.json` in your VSCODE project, add this:

``` shell
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Docker Xdebug",
      "type": "php",
      "request": "launch",
      # Put here the remote port you defined at "xdebug.remote_port" in your xdebug.ini file:
      "port": 9000,
      "pathMappings": {
        # Put here "container_path:local_path" ---> ¡IMPORTANT for BREAKPOINTS in your local code to work!
        "/var/www/html": "/home/jesus/proyectos" 
      },
      "ignore": [
        "**/vendor/**/*.php"
      ]
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

Docker containers are useful for testing your code in different PHP versions. This video shows how to use Docker to test your code in PHP 5.3, 5.4, 5.5, 5.6, 7.0, 7.1, 7.2, 7.3, 7.4, 8.0, and 8.1.
[![De PHP 5.3 a PHP 8.2](https://img.youtube.com/vi/BHAYO6esXlw/0.jpg)](https://www.youtube.com/watch?v=BHAYO6esXlw)

- [Migración de Zend Framework 1 a PHP 8.1](https://github.com/Shardj/zf1-future)
- [Terraform](https://registry.terraform.io/) a cloud management service for AWS, Google Cloud, Azure, and so on.

## Interesting Dockerfiles

- <https://github.com/PauGa9/php-symfony-mysql-nginx-docker>
- <https://github.com/thecodeholic/php-mvc-framework/blob/master/docker/php.ini>
- <https://jasonterando.medium.com/debugging-with-visual-studio-code-xdebug-and-docker-on-windows-b63a10b0dec>
- <https://github.com/thecodeholic/php-mvc-framework>
- <https://stackoverflow.com/questions/52579102/debug-php-with-vscode-and-docker>

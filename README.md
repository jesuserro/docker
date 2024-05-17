# My Docker Garden

Simple examples of using Docker. Creation of containers with PHP 7/8 and XDebug.
It is made from:

- Alpine Linux image: <https://hub.docker.com/_/alpine> ([Dockerfile.70](https://github.com/jesuserro/docker/blob/main/Dockerfile.70))
- Official PHP image: <https://hub.docker.com/_/php> ([Dockerfile.81](https://github.com/jesuserro/docker/blob/main/Dockerfile.81))

These Dockerfiles are based on the official images, but they have been modified to install XDebug and other extensions.

## Installation

``` shell
mkdir ~/proyectos && cd ~/proyectos
git clone https://github.com/jesuserro/docker.git
```

## File Structure

``` shell
📦home
 ┣ 📂jesus
 ┃ ┗ 📂proyectos
 ┃ ┃ ┗ 📂docker
 ┃ ┃ ┗ 📂project-1
 ┃ ┃ ┗ 📂ofertas
 ┃ ┃ ┗ 📂nges
```

## Usage

### Run containers

``` shell
cd ~/proyectos/docker

# Run/Launch container for php 7.0
docker compose up
docker-compose docker-compose.dev.yml up

# Run/Launch all containers for php 7.0 and php 8
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up
```

### In your browser

- <http://localhost:8070>
- <http://localhost:8081>
- <http://localhost:8070/ofertas/public/admin/index.php>
- <http://localhost:8081/nges/public/>

### Regenerating containers

``` shell
# Create new image called "my-php-image70" from existing "Dockerfile.70"
docker build -t my-php-image70 -f Dockerfile.70 .
docker build -t my-php-image81 -f Dockerfile.81 .

# Run new container instance called "my-php-container70" based on existing image "my-php-image70"
docker run -d -p 8070:80 --name my-php-container70 my-php-image70 -v /home/jesus/proyectos:/var/www/html
docker run -d -p 8081:80 --name my-php-container81 my-php-image81 -v /home/jesus/proyectos:/var/www/html
```

### Other commands

``` shell
# Start terminal session inside the "my-php-container70" container
docker exec -it my-php-container70 /bin/bash

# Show container logs
docker logs my-php-container70

# Stopping a container
docker stop my-php-container70

# Removing all docker containers  
docker container rm -f $(docker container ls -aq)

# Removing all docker images  
docker rmi $(docker images -q)

# Removing all in 1 sentence
docker container rm -f $(docker container ls -aq) && docker rmi $(docker images -q)

# Full remove
docker system prune -a

# Borrado total de la caché
docker builder prune -a
```

## XDEBUG

In your WSL2 terminal, run this command to get your IP address:

``` shell
ip addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'
  # Returning: 172.20.28.159
```

### XDebug 2.5.5/PHP 7.0

So put it at the `xdebug.ini` file:

``` shell
xdebug.remote_host = 172.20.28.159
```

### XDebug 3/PHP 8+

No need IP definition, so put it at the `xdebug.ini` file:

``` shell
xdebug.remote_host=host.docker.internal
```

and in the `docker-compose.dev.yml` file:

``` shell
  environment:
    XDEBUG_MODE: develop,debug
    XDEBUG_CONFIG:
      client_host=host.docker.internal
      start_with_request=yes
```

## VSCODE

If the IP is correct, you should see the Call Stack requests in the Debug Pane at VSCODE.
Now, in the `launch.json` in your VSCODE project, add this:

``` shell
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Listen for Xdebug",
      "type": "php",
      "request": "launch",
      "port": 9003
    },
    {
      "name": "Docker Xdebug",
      "type": "php",
      "request": "launch",
      # Put here the remote port you defined at "xdebug.remote_port" in your xdebug.ini file:
      "port": 9000, # 9000 for PHP 7.0 (XDebug 2.5.5) and 9003 for PHP 8 (XDebug 3)
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

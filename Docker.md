# Docker Giga CheatSheet
---

## Comandos de Linux

## Comandos de Docker

### Modelo DockerFile

```
FROM alpine

LABEL maintainer="email@gmail.com" #-> Actualizamos e instalamos paquetes con APK para Alpine

RUN apk update && apk add apache2 php php-apache2 openrc tar #-> Copiamos script para lanzar Apache 2

ADD ./start.sh /start.sh #-> Descargamos un ejemplo de <?php phpinfo(); ?> por enseñar como bajar algo de Internet

# Podría haber sido simplemente: RUN echo "<?php phpinfo(); ?>" > /var/www/localhost/htdocs/index.php

ADD https://gist.githubusercontent.com/SyntaxC4/5648247/raw/94277156638f9c309f2e36e19bff378ba7364907/info.php
/var/www/localhost/htdocs/index.php 

# Si quisiéramos algo como Wordpress haríamos: ADD http://wordpress.org/latest.tar.gz /var/www/localhost/htdocs/wordpress.tar.gz
# RUN tar xvzf /var/www/localhost/htdocs/wordpress.tar.gz && rm -rf /var/www/localhost/htdocs/wordpress.tar.gz
# Usamos usuario y grupo www-data. El grupo lo crea Apache, pero si quisiéramos crear grupo
# Grupo www-data RUN set -x && addgroup -g 82 -S www-data


RUN adduser -u 82 -D -S -G www-data www-data #-> Creamos usuario www-data y lo añadimos a ese grupo
# Hacemos todos los ficheros de /var/www propiedad de www-data y damos permisos a esos ficheros y a start.sh
RUN chown -R www-data:www-data /var/www/ && chmod -R 775 /var/www/ && chmod 755 /start.sh

#Indicamos puerto a exponer (para otros contenedores) 80
EXPOSE 80

#Comando lanzado por defecto al instalar el contendor
CMD /start.sh
```
---
## Comandos DockerCompose
> docker compose up -d
- Inicia el sistema definido en “docker-compose.yml” en segundo plano. Genera y descarga imágenes requeridas.

>docker compose down
- Detiene y elimina los contenedores según la configuración de “docker-compose.yml”.

>docker compose build/pull
- Construye/descarga las imágenes de contenedores según la configuración de “docker-compose.yml”.

>docker compose ps
- Muestra información de los contenedores según la configuración de “docker-compose.yml”.

>docker compose up -d --scale web=3
- Similar a “docker compose up -d” solo que además, el servicio definido como “web” en el fichero “dockercompose.yml” lo escala creando 3 copias y realizando balanceo automático si se realiza una petición al host
llamado como el servicio “web”.

### Modelo DockerCompose

Estos deben estar identados al dedillo, sino generará problemas porque faltan propiedades en alguno de los lugares.
```
version: "3.9"  # Versión de la sintaxis de docker-compose que se está utilizando.

    services:
        db:
        image: mariadb:10.11.2  # Imagen de Docker de MariaDB versión 10.11.2.
        volumes:
            - db_data:/var/lib/mysql  # Volumen de datos de la base de datos para persistencia.
        restart: always  # Reinicia el contenedor automáticamente si falla.
        environment:
            MARIADB_ROOT_PASSWORD: somewordpress  # Contraseña de root para MariaDB.
            MARIADB_DATABASE: wordpress           # Nombre de la base de datos a crear.
            MARIADB_USER: wordpress               # Usuario de MariaDB que tendrá acceso a la base de datos.
            MARIADB_PASSWORD: wordpress           # Contraseña del usuario de la base de datos.


        wordpress:
        depends_on:
            - db  # Indica que este servicio debe esperar a que el servicio "db" esté listo.
        image: wordpress:latest  # Imagen de Docker de WordPress, con la última versión.
        ports:
            - "8000:80"  # Expone el puerto 80 del contenedor en el puerto 8000 del host.
        restart: always  # Reinicia el contenedor automáticamente si falla.
        environment:
            WORDPRESS_DB_HOST: db:3306         # Host de la base de datos y el puerto.
            WORDPRESS_DB_USER: wordpress       # Usuario de la base de datos (coincide con el definido en el servicio "db").
            WORDPRESS_DB_PASSWORD: wordpress   # Contraseña de la base de datos.
            WORDPRESS_DB_NAME: wordpress       # Nombre de la base de datos.

volumes:
    db_data:

```
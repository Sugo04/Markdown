# Docker Giga CheatSheet
---
(Aclaro por si acaso que primero va el comando y luego su descripción, gracias por leer.)

---
## Comandos de Docker

### Docker ps
> docker ps

El comando docker ps proporciona una tabla con información sobre cada contenedor en ejecución. Los campos más
comunes que muestra son:
- **CONTAINER ID**: El identificador único del contenedor.

- **IMAGE**: La imagen de Docker utilizada para crear el contenedor.

- **COMMAND**: El comando que se está ejecutando en el contenedor.

- **CREATED**: El tiempo que ha pasado desde que se creó el contenedor.

- **STATUS**: El estado actual del contenedor.

- **PORTS**: Los puertos en los que está expuesto el contenedor.

- **NAMES**: El nombre asignado al contenedor.

> docker ps -a

El comando docker ps -a proporciona una tabla con información sobre todos los contenedores, incluyendo aquellos que están detenidos. Muestra lo mismo que ```docker ps```.

### Docker Run
> docker run -it --name=cont1 ubuntu /bin/bash
- docker run: Inicia un nuevo contenedor.
- -it: Combina dos opciones:
    - -i (interactivo) permite que se mantenga la entrada estándar abierta.
    - -t (pseudo-TTY) asigna una pseudo-terminal al contenedor.
- --name=cont1: Asigna el nombre cont1 al contenedor.
- ubuntu: Utiliza la imagen de Ubuntu para crear el contenedor. Si no tienes la imagen
localmente, Docker la descargará del repositorio Docker Hub.
- /bin/bash: El comando que se ejecuta dentro del contenedor, en este caso, el shell
Bash.

> docker run -d -p 1200:80 nginx
- docker run: Inicia un nuevo contenedor.
- -d: Ejecuta el contenedor en segundo plano (modo "detached").
- -p 1200:80: Mapea el puerto 1200 del host al puerto 80 del contenedor. Esto significa que
podrás acceder al servidor Nginx desde el host a través del puerto 1200.
- nginx: Utiliza la imagen de Nginx para crear el contenedor. Si no tienes la imagen localmente,
Docker la descargará del repositorio Docker Hub.


> docker run -it -e MENSAJE=HOLA ubuntu:14.04 bash

El comando que has proporcionado crea y ejecuta un contenedor Docker interactivo basado en la imagen de Ubuntu
14.04, asignándole una variable de entorno llamada MENSAJE con el valor HOLA, y ejecuta el shell bash dentro del
contenedor. A continuación, te explico cada parte del comando:

- docker run: Inicia un nuevo contenedor.

- -it: Combina dos opciones:

    - -i (interactivo) permite que se mantenga la entrada estándar abierta.

    - -t (pseudo-TTY) asigna una pseudo-terminal al contenedor.

- -e MENSAJE=HOLA: Define una variable de entorno llamada MENSAJE con el valor HOLA dentro del
contenedor.

- ubuntu:14.04: Utiliza la imagen de Ubuntu en su versión 14.04 para crear el contenedor. Si no
tienes la imagen localmente, Docker la descargará del repositorio Docker Hub.

- bash: El comando que se ejecuta dentro del contenedor, en este caso, el shell Bash.

Al ejecutar este comando, tendrás una sesión interactiva de Bash dentro de un contenedor de Ubuntu
14.04 con la variable de entorno MENSAJE establecida en HOLA. Puedes usar esta sesión para instalar
software, realizar pruebas u otras tareas.

### Docker Start/Stop/Restart
> docker start micontenedor

El comando docker start se utiliza para iniciar un contenedor Docker que ha sido previamente detenido. 

> docker start -ai micontenedor

El comando docker start -ai micontenedor se utiliza para iniciar un contenedor detenido y adjuntar la
terminal de manera interactiva a su salida estándar.

- -a: Adjunta la terminal del host a la salida estándar (stdout) y a la entrada estándar (stdin) del contenedor.

- -i: Abre la terminal de manera interactiva, permitiendo la entrada de comandos.

- micontenedor: El nombre o ID del contenedor que deseas iniciar.

Se utiliza para reiniciar un contenedor existente que ha sido detenido, recuperando su estado y
permitiendo la interacción. 

### Docker exec
> docker exec -it -e FICHERO=prueba cont bash

El comando docker exec se utiliza para ejecutar un nuevo comando en un contenedor que ya está en ejecución. La combinación docker exec -it -e FICHERO=prueba cont bash ejecuta un shell Bash dentro del contenedor cont y establece una variable de entorno llamada FICHERO con el valor prueba.

 ```docker exec```: Ejecuta un nuevo comando en un contenedor que ya está en ejecución.

- -it: Combina dos opciones:

    - -i (interactivo): Mantiene la entrada estándar abierta.

    - -t (pseudo-TTY): Asigna una pseudo-terminal al comando.

- -e FICHERO=prueba: Define una variable de entorno llamada FICHERO con el valor prueba que estará disponible solo en el contexto del comando que se ejecuta.

- cont: El nombre o ID del contenedor en el que se ejecutará el comando.

- bash: El comando a ejecutar dentro del contenedor. En este caso, abre un shell Bash.

> docker exec -d cont touch /tmp/prueba

El comando docker exec -d cont touch /tmp/prueba se utiliza para ejecutar un comando dentro de un contenedor en ejecución (cont) de forma desacoplada, es decir, en segundo plano.

```docker exec```: Ejecuta un nuevo comando en un contenedor que ya está en ejecución.

- -d: Ejecuta el comando en modo desacoplado (detached mode), es decir, en segundo plano.

- cont: El nombre o ID del contenedor en el que se ejecutará el comando.

- touch /tmp/prueba: El comando que se ejecutará dentro del contenedor. En este caso, el comando touch crea un archivo vacío (o actualiza la marca de tiempo si el archivo ya existe) en la ruta /tmp/prueba.

### Docker attach
> docker attach idcontainer

El comando docker attach idcontainer se usa para adjuntar tu terminal a un contenedor Docker que ya está en ejecución. Esto te permite interactuar con los procesos que se están ejecutando dentro del contenedor como si estuvieras trabajando directamente en su terminal.

- docker: Este es el comando principal para interactuar con Docker.

- attach: Este subcomando indica que deseas adjuntar tu terminal a un contenedor en ejecución.

- idcontainer: Esto debe ser reemplazado por el ID o el nombre del contenedor al que deseas adjuntarte.

### Docker logs
> docker logs -n 10 idcontainer

Muestra las 10 últimas líneas de la salida estándar producida por el proceso en ejecución en el contendor.

### Docker cp
> docker cp idcontainer:/tmp/prueba ./

Copia el fichero “/tmp/prueba” del contenedor “idcontainer” al directorio actual del anfitrión.

> docker cp ./miFichero idcontainer:/tmp

Copia el fichero “miFichero” del directorio actual del anfitrión a la carpeta “/tmp” del contenedor

### Gestion de Images

> docker images

Información de imágenes locales disponibles.

> docker search ubuntu

Busca la imagen “ubuntu” en el repositorio remoto (por defecto Docker Hub).

> docker pull alpine

 Descarga localmente imagen “alpine”

> docker history alpine

Muestra la historia de creación de la imagen “alpine”.

> docker rmi ubuntu:14.04

Elimina localmente la imagen “ubuntu” con tag “14.04”

> docker rmi $(docker images -q)

Borra toda imagen local que no esté siendo usada por un contenedor
> docker rm IDCONTENEDOR

Borra un contenedor con IDCONTENEDOR.

> docker stop $(docker ps -a -q)

Para todos los contenedores del sistema.

> docker rm $(docker ps -a -q)

Borra todos los contenedores parados del sistema.

> docker system prune -a

Borra todas las imágenes y contenedores parados del sistema.

### Creación de Imágenes a partir de contenedores

> docker commit -m "comentario" IDCONTENEDOR usuario/imagen:version

Hace commit de un contenedor existente a una imagen local.

> docker save -o copiaSeguridad.tar imagenA

Guarda una copia de seguridad de una imagen en fichero “.tar”

> docker load -i copiaSeguridad.tar

Restaura una copia de seguridad de una imagen en fichero “.tar”.

### Gestión de Redes

> docker network create redtest

Creamos la red “redtest”

>docker network ls

Nos permite ver el listado de redes existentes.

> docker network rm redtest

Borramos la red “redtest”.

> docker run -it --network redtest ubuntu /bin/bash

Conectamos el contenedor que creamos a la red “redtest”.

> docker network connect IDRED IDCONTENEDOR

Conectamos un contenedor a una red.

> docker network disconnect IDRED IDCONTENEDOR

Desconectamos un contenedor de una red

### Volúmenes

> docker run -d -it --name appcontainer -v /home/sergi/target:/app nginx:latest

Creamos un contenedor y asignamos un volumen con “binding mount”.

> docker run -d -it --name appcontainer -v micontenedor:/app nginx:latest

Creamos un contenedor y asignamos un volumen Docker llamado “micontenedor”.

> docker volume create/ls/rm mivolumen

Permite crear, listar o eliminar volúmenes Docker

> docker run -d -it -tmpfs /app nginx

Permite crear un contenedor y asociar un volumen “tmpfs”.

> docker run --rm --volumes-from contenedor1 -v /home/sergi/backup:/backup ubuntu bash -c "cd /datos && tar cvf /backup/copiaseguridad.tar ."

Permite realizar una copia de seguridad de un volumen asociado a “contenedor1” y que se monta en “/datos”. Dicha copia finalmente acabará en “/home/sergi/backup” de la máquina anfitrión

> docker volume rm $(docker volume ls -q)

Permite eliminar todos los lúmenes de tu máquina.

### Docker Hub

> docker login

Permite introducir credenciales del registro (por defecto “Docker Hub”)

> docker push usuario/imagen:version

Permite subir al repositorio una imagen mediante “push”.

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
---

## Comandos de Linux
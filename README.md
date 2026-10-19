# Tarea Módulo 2: Ejecutando y gestionando contenedores

**Alumno:** Badr Diboune El Wasmi

## 1. Descarga de imágenes Docker

En este apartado descargué varias imágenes de Docker para utilizarlas posteriormente en los contenedores.

```powershell
docker pull ubuntu:18.04
docker pull centos:8
docker pull debian:9
docker pull mariadb:latest
docker pull mysql:5.7
docker pull httpd
docker pull tomcat:9.0.39-jdk11
docker pull jenkins/jenkins:lts
docker pull php:7.3-apache
```

Para comprobar que se han descargado correctamente, utilicé:

```powershell
docker images
```

## 2. Arrancar y gestionar un contenedor

Creé un contenedor llamado `ubuntu` utilizando la imagen Ubuntu 18.04.

### Crear el contenedor

```powershell
docker run -dit --name ubuntu ubuntu:18.04
```

- `run`: crea y arranca el contenedor.
- `-d`: lo ejecuta en segundo plano.
- `-i` y `-t`: permiten mantener una terminal interactiva.
- `--name ubuntu`: asigna el nombre al contenedor.

Para comprobar que está funcionando:

```powershell
docker ps
```

### Parar el contenedor

```powershell
docker stop ubuntu
```

Para comprobar que está parado:

```powershell
docker ps -a
```

### Volver a arrancar el contenedor

```powershell
docker start ubuntu
```

### Consultar información del sistema operativo

Para mostrar el contenido del archivo `/etc/os-release` sin entrar en el contenedor:

```powershell
docker exec ubuntu cat /etc/os-release
```

## 3. Ejecutar un servidor web con PHP y Apache

Creé un contenedor llamado `web` con la imagen `php:7.3-apache`, utilizando el puerto 8181 del equipo.

```powershell
docker run -d --name web -p 8181:80 php:7.3-apache
```

### Crear el archivo index.html

Ejecuté el siguiente comando en PowerShell para crear la página web:

```powershell
Set-Content -Path index.html -Value '<h1>HOLA SOY BADR DIBOUNE EL WASMI</h1>' -Encoding utf8
```

Después eliminé el contenedor y lo volví a crear compartiendo la carpeta actual con el directorio web de Apache:

```powershell
docker rm -f web
docker run -d --name web -p 8181:80 -v "${PWD}:/var/www/html" php:7.3-apache
```

Para comprobar que funciona, accedí desde el navegador a:

http://localhost:8181/

### Crear el archivo index.php

Creé un archivo PHP para comprobar la información de PHP:

```powershell
Set-Content -Path index.php -Value '<?php phpinfo(); ?>' -Encoding utf8
```

Accedí desde el navegador a:

http://localhost:8181/index.php

La página muestra información sobre la versión y la configuración de PHP.

## 4. Ejecutar una base de datos MariaDB

Creé un contenedor llamado `bbdd` utilizando la imagen `mariadb:latest`.

La configuración solicitada fue la siguiente:

- Nombre del contenedor: `bbdd`
- Contraseña de root: `root`
- Base de datos: `prueba`
- Usuario: `invitado`
- Contraseña del usuario: `invitado`
- Puerto del equipo: `3336`

Para crear el contenedor utilicé:

```powershell
docker run -d --name bbdd -p 3336:3306 -e MARIADB_ROOT_PASSWORD=root -e MARIADB_DATABASE=prueba -e MARIADB_USER=invitado -e MARIADB_PASSWORD=invitado mariadb:latest
```

Para comprobar las bases de datos con el usuario `invitado`:

```powershell
docker exec -it bbdd mariadb -u invitado -pinvitado -e "SHOW DATABASES;"
```

El resultado muestra la base de datos `prueba` y la base de datos interna `information_schema`.

## 5. Obtener información de los contenedores

### Dirección IP del contenedor web

```powershell
docker inspect web --format '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'
```

### Puertos del contenedor web

```powershell
docker port web
```

El puerto 80 del contenedor está publicado en el puerto 8181 del equipo.

### Dirección IP del contenedor bbdd

```powershell
docker inspect bbdd --format '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'
```

### Puertos del contenedor bbdd

```powershell
docker port bbdd
```

El puerto 3306 del contenedor está publicado en el puerto 3336 del equipo.

## 6. Conclusión

Con esta práctica aprendí a descargar imágenes de Docker, crear y gestionar contenedores, parar y volver a arrancarlos, ejecutar un servidor web con PHP y Apache y configurar una base de datos MariaDB. También aprendí a consultar las direcciones IP y los puertos publicados de los contenedores.

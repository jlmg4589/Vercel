---
{"dg-publish":true,"permalink":"/optativa-desarrollo-web-full-stack/apuntes/tema-00-repaso/apendice-2-virtualizacion-con-docker/","title":"Apéndice 2. Virtualización con Docker","tags":["clippings"],"noteIcon":"","created":"2026-09-27T13:23:21.680+02:00","updated":"2026-09-27T13:23:21.680+02:00","dg-note-properties":{"title":"Apéndice 2. Virtualización con Docker","source":"https://iescelia.org/docs/fullstack/_site/docker/#a24-montando-con-docker-un-servidor-web-con-persistencia-de-datos","author":["Alfredo Moreno Vozmediano"],"published":null,"created":"2026-09-20","description":"Apuntes del módulo optativo de “Desarrollo web full stack”, de 2º curso del Ciclo Formativo de Grado Superior de Desarrollo de Aplicaciones Web/Multiplataforma impartido en el IES Celia Viñas de Almería (España)","tags":["clippings"]}}
---

## A2.1. ¿Qué es Docker?

**Docker** es una herramienta de virtualización basada en _contenedores_.

Un **contenedor** es un paquete de software completamente independiente del sistema donde se ejecuta. Recibe ese nombre por los contenedores que se utilizan en el transporte marítimo, que tienen unas medidas y una forma estandarizada y que aislan por completo la carga que llevan dentro del exterior.

Un contenedor Docker hace lo mismo, pero con un conjunto de software: lo aisla por completo del exterior. El software que hay dentro del contenedor se puede ejecutar en cualquier máquina gracias al _runtime_ de Docker, que se comporta como un mini-sistema operativo virtualizado que corre sobre la máquina anfitrión.

Un contenedor puede contener cualquier cosa. Por ejemplo, Apache. De ese modo, podemos ejecutar Apache en cualquier máquina (siempre que tenga previamente instalado Docker) sin necesidad de instalarlo realmente, con todo lo que ello conlleva de configuración de la máquina, consumo de recursos, etc. El contenedor Docker puede ponerse en marcha cuando queramos y detenerse en cualquier momento, sin dejar ningún rastro en la máquina anfitriona.

En definitiva, puedes usar y/o testear _cualquier_ programa sin tener que instalarlo realmente en tu máquina.

Los contenedores Docker vienen empaquetados en **imágenes**, a partir de los cuales pueden lanzarse todos los contenedores que necesitemos. Es decir, las imágenes con como las _clases_ en programación orientada a objetos, y los contenedores son como los objetos que se instancian a partir de esas clases.

Cada cual puede construir las imágenes que necesite o usar imágenes ya hechas, con todo lo necesario en su interior para ejecutar cualquier software sin necesidad de instalarlo ni configurarlo. Hay repositorios públicos de imágenes, como **DockerHub**, donde uno puede encontrar imágenes de prácticamente cualquier cosa.

## A2.2. Comandos usuales de Docker

Aunque Docker puede usarse desde un interfaz gráfico (como **Docker Desktop**), lo habitual es hacerlo desde la línea de comandos.

Esto no es un manual de Docker, pero sí vamos a enumerar aquí los comandos principales que nos serán útiles como desarrolladores web para que puedas usarlos como referencia rápida cuando tengas que trabajar con Docker.

- `docker run [nombre-imagen]` - Lanza un contenedor a partir de la imagen especificada. Si la imagen no está descargada en el ordenador, la buscará en el repositorio configurado (por defecto, _DockerHub_).
- `docker ps` - Muestra un listado con los contenedores que hay actualmente en el sistema. Un contenedor no tiene por qué estar necesariamente corriendo, sino que existen otros estados (detenido, preparado, finalizado, etc). Con `docker ps -a` podemos ver todos los contenedores, también los detenidos.
- `docker stop [id-del-contenedor]` - Detiene un contenedor. Su id puede obtenerse con `docker ps`.
- `docker start [id-del-contenedor]` - Reanuda un contenedor.
- `docker exec -it [id-del-contenedor] bash` - Abrir un terminal en el contenedor.
- `docker-compose up -d` - Inicia todos los contenedores especificados en el archivo _docker-compose.yml_ del directorio actual. Necesitas tener instalado, además de Docker, el programa _docker-compose_.
- `docker-compose down` - Detiene todos los contenedores especificados en el archivo _docker-compose.yml_ del directorio actual.

## A2.3. Persistencia de datos

Cualquier cosa que guardes en un contenedor de Docker se perderá cuando el contenedor se detenga. Por ejemplo, si estás haciendo una aplicación web que usa una base de datos MySQL, y tu servidor MySQL está en un contenedor Docker, toda la información de esa base de datos se perderá cada vez que destruyas el contenedor.

Es posible evitar eso usando la persistencia de datos. Consiste en pedirle a Docker que guarde datos _fuera_ del contenedor, para que estos no se pierdan al reiniciarlo o eliminarlo. Por ejemplo, los datos de la base de datos.

La persistencia se puede habilitar con **docker run**. Por ejemplo:

```
$ docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=clave123 -v mysql-data:/var/lib/mysql mysql:latest
```

La persistencia se habilita con _-v mysql-data:/var/lib/mysql_, que crea (o usa) un volumen llamado _mysql-data_ y lo monta en la ruta _/var/lib/mysql_ de la máquina real, que es donde MySQL suele guardar los datos.

Personalmente, encuentro más sencillo hacerlo todo a través de **docker-compose**. Por ejemplo, mira este archivo de configuración docker-compose.yml:

```
services:
  mysql:
    image: mysql:latest
    container_name: mysql-container
    volumes:
      - mysql-data:/var/lib/mysql
    ports:
      - "3306:3306"

volumes:
  mysql-data:
```

Con este archivo de configuración se lanzará un contenedor de _mysql_ en cuanto tecleemos _docker-compose up_. Observa estas dos líneas:

- La línea _volumes_ dentro del servicio _mysql_ monta el volumen virtual _mysql-data_ en _/var/lib/mysql_, el lugar de la máquina real donde MySQL suele guardar los datos.
- La línea _volumes_ al final del archivo declara el volumen llamado _mysql-data_, que Docker creará si no existe.

Así, los datos persisten en _/var/lib/mysql_ aunque detengas o elimines el contenedor.

## A2.4. Montando con Docker un servidor web con persistencia de datos

En esta sección vamos a mostrar cómo montar un servidor web con imágenes Docker y levantarlo o apagarlo con docker-compose.

Usaremos las imágenes oficiales de cada desarrollador, y necesitaremos poner en marcha **cuatro contenedores** simultáneamente, por lo que será mucho más cómodo hacerlo con docker-compose para poder levantarlas todas a la vez y no de una en una:

1. **Servidor Apache**
2. **Intérprete PHP**
3. **Servidor MariaDB**
4. **PHPMyAdmin**

Además, necesitamos que los datos de MariaDB sean persistentes, es decir, que no se pierdan cuando detengamos los contenedores.

Lograr esto es complicadillo, pero trabajar con los servidores siempre lo es. A cambio, tendremos un entorno fácilmente transportable a otros servidores.

Te dejo las instrucciones paso a paso para que lo consigas sin desesperarte demasiado:

### Paso 1. Crear ./docker-compose.yml

Crea un archivo **_docker-compose.yml_** en tu directorio de trabajo con este contenido exacto, para trabajar con las imágene de nuestros cuatro servicios.

Usaremos solo las imágenes oficiales. Algunas las tendremos que modificar un poco para que sirvan a nuestros propósitos: para eso montaremos los archivos _custom.ini_ (en la imagen de PHP) y _httpd.conf_ y _myapp.conf_ (en la imagen de Apache).

```yaml
services:
  php:
    build: .  
    volumes:
      - ./app:/usr/local/apache2/htdocs
      - ./custom.ini:/usr/local/etc/php/conf.d/custom.ini
    depends_on:
      - mariadb

  apache:
    image: httpd:2.4
    ports:
      - "8080:80"
    volumes:
      - ./app:/usr/local/apache2/htdocs
      - ./apache-custom/httpd.conf:/usr/local/apache2/conf/httpd.conf:ro
      - ./apache-custom/myapp.conf:/usr/local/apache2/conf/extra/myapp.conf:ro
    depends_on:
      - php

  mariadb:
    image: mariadb:10.6
    environment:
      MYSQL_ROOT_PASSWORD: 1234
      MYSQL_DATABASE: pruebas
      MYSQL_USER: user
      MYSQL_PASSWORD: 1234
    volumes:
      - mariadb_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8000:80"
    environment:
      PMA_HOST: mariadb
    depends_on:
      - mariadb

volumes:
  mariadb_data:
```

### Paso 2. Crear un ./Dockerfile para PHP

La imagen oficial de PHP-FPM es muy minimalista y viene con muy pocas extensiones instaladas. Tenemos que instalar algunas librerías adicionales en ese contenedor (como _pdo_mysql_ para acceso a bases de datos MySQL con PDO).

Esto se logra creando un archivo llamado **_Dockerfile_** en el directorio raíz (en la misma carpeta donde tengas _docker-compose.yml_) con este contenido:

```
FROM php:8.2-fpm

# Instalar dependencias necesarias
RUN apt-get update && apt-get install -y \
        default-mysql-client \
        libzip-dev \
        unzip \
        git \
    && docker-php-ext-install pdo pdo_mysql mysqli zip \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
```

Así informamos a Docker de que:

- a) Queremos usar _php:8.2-fpm_, la imagen oficial de PHP (versión 8.2) como base para el contenedor.
- b) Queremos instalar (con el comando _RUN apt-get_) varias extensiones útiles al levantar el contenedor por primera vez (puede tardar un poco).
- 
### Paso 3. Crear ./apache-custom/httpd.conf

Por defecto, la imagen oficial de Apache no interpreta el código PHP, sino que lo sirve en texto plano, como si fuera HTML.

Podemos reconfigurar esta imagen sin tener que meter mano también a este contenedor:

1. Crea el directorio **_apache-custom_** en tu carpeta de trabajo.
2. Crea el archivo **_./apache-custom/httpd.conf_** con este contenido:

```
# httpd.conf mínimo preparado para usar Apache httpd:2.4 + PHP-FPM

ServerRoot "/usr/local/apache2"

# Puerto en el que Apache escuchará dentro del contenedor
Listen 80

# Módulos esenciales
LoadModule mpm_event_module modules/mod_mpm_event.so
LoadModule authn_core_module modules/mod_authn_core.so
LoadModule authz_core_module modules/mod_authz_core.so
LoadModule unixd_module modules/mod_unixd.so
LoadModule dir_module modules/mod_dir.so
LoadModule mime_module modules/mod_mime.so
LoadModule log_config_module modules/mod_log_config.so
LoadModule env_module modules/mod_env.so
LoadModule setenvif_module modules/mod_setenvif.so
LoadModule alias_module modules/mod_alias.so
LoadModule negotiation_module modules/mod_negotiation.so
LoadModule autoindex_module modules/mod_autoindex.so
LoadModule headers_module modules/mod_headers.so

# Módulos necesarios para proxying a PHP-FPM
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so

# Información del servidor
ServerAdmin you@example.com
ServerName localhost:80

# Archivos de tipos mime
TypesConfig conf/mime.types

# Logs: enviar a stdout/stderr para que docker-compose logs funcione bien
ErrorLog /proc/self/fd/2
CustomLog /proc/self/fd/1 common

# Seguridad por defecto
<Directory />
    AllowOverride none
    Require all denied
</Directory>

# DocumentRoot por defecto
DocumentRoot "/usr/local/apache2/htdocs"
<Directory "/usr/local/apache2/htdocs">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

# Índices (queremos que index.php tenga preferencia sobre index.html)
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>

# Incluimos el virtualhost personalizado que defines en apache-custom/myapp.conf
Include conf/extra/myapp.conf

# Fin del archivo
```

### Paso 4. Crear ./apache-custom/myapp.conf

En este archivo de configuración adicional redirigiremos todas las peticiones de archivos .php hacia el contenedor con el intérprete PHP. El resto de archivos serán servidos por Apache.

Crea el archivo **_./apache-custom/myapp.conf_** con este contenido:

```
<VirtualHost *:80>
    DocumentRoot "/usr/local/apache2/htdocs"

    <Directory "/usr/local/apache2/htdocs">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
        DirectoryIndex index.php
    </Directory>

    # Enviar todas las peticiones a .php al FPM del contenedor `php`
    ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://php:9000/app/$1
</VirtualHost>
```

### Paso 5. Crear el archivo custom.ini

Este archivo contiene las **opciones de configuración adicionales para PHP**.

PHP se configura dentro del contenedor correspondiente, en un archivo llamado _php.ini_. Podemos agregar configuraciones adicionales sin necesidad de tocar el contenedor con un archivo de configuración adicional que mapearemos al interior del contenedor.

Crea un archivo llamado **_custom.ini_** en tu directorio de trabajo con este contenido:

```
display_errors = On
display_startup_errors = On
error_reporting = E_ALL
opcache.enable = 0
opcache.enable_cli = 0
output_buffering = Off
```

Esto **habilitará las opciones de depuración de errores** de PHP. En un entorno de producción, estas opciones se deshabilitarían, claro.

También deshabilitará la caché, imprescindible para que, al desarrollar, nuestros cambios se vean inmediatamente en el servidor.

Si más adelante necesitas **configuraciones adicionales para PHP** (como incrementar el tamaño máximo de archivos subidos al servidor o el tiempo de procesamiento de un script), puedes hacerlo fácilmente en este custom.ini.

### Paso 6. Levantar los contenedores con docker-compose up

Ya lo tenemos todo preparado.

Ahora podemos **poner en marcha los cuatro contenedores** tecleando (en el directorio de trabajo):

```
$ docker-compose up --build   # Lanzar contenedores la primera vez (o después cambiar docker-compose.yml o Dockerfile)
```

O bien, si no hemos tocado la configuración de los contenedores recientemente:

```
$ docker-compose up   # Lanzar contenedores habitualmente
```

También podemos lanzarlo en segundo plano, para que la consola no se quede bloqueada:

```
$ docker-compose up -d   # Lanzar contenedores en segundo plano
```

### Paso 7. Probar los contenedores

Si todo ha ido bien, deberías tener estos servicios activos:

- **http://localhost:8080** -> Aquí debería estar escuchando Apache/PHP. Si pones un archivo .php en la carpeta ./app de tu proyecto, tendría que verse el resultado.
- **http://localhost:8000** -> Aquí debería estar escuchando PHPMyAdmin. El usuario y contraseña de la base de datos están en el _docker-compose.yml_ (los puedes cambiar allí si no te gustan).

### Paso 8. Detener los contenedores

Para detener los contenedores, tan solo teclea:

```
$ docker-compose down
```

O bien pulsa **CTRL + C** si inciaste docker-compose en segundo plano (con la opción -d).
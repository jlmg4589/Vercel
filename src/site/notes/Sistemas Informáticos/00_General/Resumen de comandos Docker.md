---
{"dg-publish":true,"permalink":"/sistemas-informaticos/00-general/resumen-de-comandos-docker/","tags":["asignatura/sistemas-informaticos","docker","comandos","chuleta"],"noteIcon":"","dg-note-properties":{"tipo":"recurso","asignatura":"Sistemas Informáticos","tags":["asignatura/sistemas-informaticos","docker","comandos","chuleta"]}}
---

# 🐳 Resumen de comandos Docker

↑ [[Sistemas Informáticos/Sistemas Informáticos\|Sistemas Informáticos]]

> [!info] Contexto
> Chuleta de comandos de Docker que se va ampliando a medida que avanzo en el curso (Docker de Antonio Corbalán). Cada comando incluye una explicación y ejemplos.

> [!tip] Idea clave
> - `docker run` parte de una **imagen** → **crea** un contenedor nuevo y lo arranca.
> - `docker start` parte de un **contenedor** existente → lo **reenciende** tal como estaba.
> - `docker exec` entra en un contenedor que **ya está corriendo** para ejecutar algo.

---

## Información del sistema

### `docker --version`
Muestra la versión de Docker instalada.
```bash
docker --version        # Docker version 29.9.0, build f415da8
```

### `docker info`
Muestra información del sistema Docker (contenedores, imágenes, versión del servidor, registros...). El contenido varía según la versión.
```bash
docker info
```

---

## Imágenes

### `docker pull`
Descarga una imagen desde un registro (por defecto, Docker Hub / `docker.io`) sin crear ningún contenedor.
```bash
docker pull nginx       # descarga docker.io/library/nginx:latest
```

---

## Ciclo de vida de contenedores

### `docker run`
**Crea un contenedor nuevo** a partir de una imagen y lo arranca. Es el único momento en que se configura (puertos, volúmenes, variables...). Por defecto arranca en **primer plano**.
```bash
docker run nginx                        # crea y arranca en primer plano
docker run -d nginx                     # -d: segundo plano (detached)
docker run -it ubuntu bash              # -it: terminal interactiva
docker run --name web -p 8080:80 nginx  # con nombre y puerto publicado
docker run --rm alpine echo hola        # --rm: se borra solo al terminar
```

### `docker start`
**Arranca un contenedor que ya existe** y está parado. Conserva su configuración y sus datos. Por defecto arranca en **segundo plano**. No admite `-p`, `-e`, `-v` (para cambiar eso hay que recrear con `run`).
```bash
docker start web         # arranca en segundo plano
docker start -a web      # -a (attach): ver su salida
docker start -ai mi_ub   # -ai: adjuntar + interactivo
```

### `docker exec`
Ejecuta un comando **dentro de un contenedor que ya está corriendo**. No crea nada nuevo.
```bash
docker exec -it web bash         # abrir una terminal dentro
docker exec web ls /etc          # lanzar un comando suelto
```

### `docker stop`
Detiene un contenedor en marcha (el contenedor sigue existiendo, solo se para).
```bash
docker stop web
```

### 📊 Tabla rápida: `run` vs `start`

| Quiero... | Con `run` (contenedor nuevo) | Con `start` (contenedor existente) |
|---|---|---|
| Arrancar en primer plano | `docker run nginx` (por defecto) | `docker start -a web` |
| Arrancar en segundo plano | `docker run -d nginx` | `docker start web` (por defecto) |
| Arrancar interactivo | `docker run -it ubuntu bash` | `docker start -ai mi_ubuntu` |
| Publicar un puerto | `docker run -p 8080:80 nginx` | ❌ hay que recrear |
| Variable de entorno | `docker run -e VAR=valor ...` | ❌ hay que recrear |
| Montar un volumen | `docker run -v datos:/app ...` | ❌ hay que recrear |
| Ponerle nombre | `docker run --name web nginx` | ya lo tiene de cuando se creó |

---

## Consulta y gestión

### `docker ps`
Lista contenedores.
```bash
docker ps        # solo los que están en marcha
docker ps -a     # todos, incluidos los parados (Exited)
```

### `docker logs`
Muestra la salida (logs) de un contenedor, es decir, lo que el proceso principal escribe en `stdout`/`stderr`.
```bash
docker logs web              # todos los logs hasta ahora
docker logs -f web           # -f (follow): seguir en directo (Ctrl+C para salir)
docker logs --tail 50 web    # solo las últimas 50 líneas
docker logs -t web           # -t: añade marca de tiempo a cada línea
docker logs --since 10m web  # solo lo registrado en los últimos 10 minutos
docker logs --since 2024-01-01T00:00:00 web   # desde una fecha/hora concreta
docker logs --until 5m web   # hasta hace 5 minutos
```
Combinaciones habituales:
```bash
docker logs -f --tail 20 web   # ver las últimas 20 líneas y seguir en directo
```
> [!note]
> `docker logs` solo funciona con el driver de logs por defecto (`json-file` o `local`). Si el contenedor envía los logs a otro sitio (p. ej. `syslog`), este comando no mostrará nada.

### `docker rename`
Cambia el nombre de un contenedor existente.
```bash
docker rename web web_viejo
```

### `docker rm`
Borra un contenedor (debe estar parado, o usar `-f`).
```bash
docker rm web           # borrar un contenedor parado
docker rm -f web        # forzar: lo para y lo borra
docker rm prueba otro   # varios a la vez
```

### `docker inspect`
Muestra información detallada (en JSON) de un contenedor o imagen.
```bash
docker inspect web
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web   # solo la IP
```

### `docker update`
Modifica ciertos parámetros de un contenedor **existente** (CPU, memoria, política de reinicio).
```bash
docker update --restart always web
```

> [!info] Redes
> Los comandos de red (`docker network ...`, publicar puertos, `--network`) tienen su propia sección: ver [[Sistemas Informáticos/00_General/Resumen de comandos Docker#🌐 Redes y puertos\|#🌐 Redes y puertos]].

### `docker cp`
Copia archivos o carpetas **entre tu máquina (host) y un contenedor**, en los dos sentidos. El contenedor puede estar incluso parado. La sintaxis es `docker cp ORIGEN DESTINO`, usando `contenedor:/ruta` para el lado del contenedor.
```bash
# Del host AL contenedor
docker cp index.html web:/usr/share/nginx/html/     # copia un archivo
docker cp ./sitio web:/usr/share/nginx/html/        # copia una carpeta entera

# Del contenedor AL host
docker cp web:/etc/nginx/nginx.conf ./nginx.conf    # extraer un archivo
docker cp web:/var/log/nginx ./logs                 # extraer una carpeta
```
> [!note]
> Es útil para sacar logs o configuraciones de un contenedor, o meter un archivo puntual. Para datos que deban **persistir**, usa **volúmenes** (`-v`) en lugar de `docker cp`.

### `docker stats`
Muestra en **tiempo real** el consumo de recursos de los contenedores (CPU, memoria, red, disco). Ojo: el comando es `stats`, en plural. Se actualiza solo hasta que pulses `Ctrl+C`.
```bash
docker stats              # todos los contenedores en marcha, en directo
docker stats web          # solo un contenedor concreto
docker stats --no-stream  # una sola lectura y sale (no se queda en directo)
```
Columnas principales: `CPU %`, `MEM USAGE / LIMIT`, `MEM %`, `NET I/O` (red), `BLOCK I/O` (disco), `PIDS` (número de procesos).

---

## 🎛️ Limitar recursos de un contenedor

Por defecto un contenedor puede usar **toda** la CPU y memoria de la máquina. Para evitar que uno se "coma" los recursos del host, se ponen límites. Estas son **opciones de `docker run`** (se fijan al crear el contenedor); en uno ya existente se pueden ajustar con `docker update`.

### Límites de memoria (RAM)
```bash
docker run -d --name web --memory="512m" nginx          # máximo 512 MB de RAM
docker run -d --memory="1g" --memory-swap="2g" nginx    # 1 GB RAM + 1 GB de swap
```
- `--memory` (o `-m`): tope de memoria RAM. Si el contenedor lo supera, Docker lo mata (OOM).
- `--memory-swap`: RAM + swap total. Si vale igual que `--memory`, se desactiva el swap.

### Límites de CPU
```bash
docker run -d --name web --cpus="1.5" nginx        # como máximo 1,5 núcleos
docker run -d --cpus="0.5" nginx                   # medio núcleo
docker run -d --cpuset-cpus="0,1" nginx            # solo puede usar los núcleos 0 y 1
docker run -d --cpu-shares=512 nginx               # prioridad relativa (1024 = por defecto)
```
- `--cpus`: cuántos núcleos (en total) puede usar. Lo más habitual.
- `--cpuset-cpus`: a qué núcleos **concretos** se limita.
- `--cpu-shares`: peso/prioridad cuando hay competencia por la CPU (no es un tope duro).

### Otros límites útiles
```bash
docker run -d --pids-limit=100 nginx    # máximo 100 procesos (evita fork bombs)
```

### Cambiar límites en un contenedor existente
```bash
docker update --memory="256m" --cpus="1" web
```

> [!tip]
> Comprueba el efecto con `docker stats`: en la columna `MEM USAGE / LIMIT` verás reflejado el tope que has puesto.

---

## 🏗️ Crear imágenes personalizadas

Hay dos caminos para fabricar tu propia imagen: con un **Dockerfile** (recomendado, reproducible) o con **`docker commit`** (rápido pero poco mantenible).

### Imágenes y su gestión

#### `docker images`
Lista las imágenes que tienes descargadas/creadas.
```bash
docker images            # todas
docker images nginx      # filtrar por nombre
```

#### `docker rmi`
Borra una imagen (debe no estar usada por ningún contenedor, o usar `-f`).
```bash
docker rmi nginx:latest
docker rmi -f mi_imagen
```

#### `docker tag`
Pone un nombre/etiqueta (o un alias) a una imagen. El formato es `nombre:etiqueta`.
```bash
docker tag mi_imagen miusuario/mi_imagen:1.0
```

#### `docker history`
Muestra las **capas** que forman una imagen y cómo se creó cada una (la instrucción del Dockerfile y el tamaño que añadió). Útil para entender de qué está hecha una imagen y qué la hace grande.
```bash
docker history nginx
docker history --no-trunc mi_imagen     # sin recortar el texto de cada capa
docker history -H=false mi_imagen       # tamaños en bytes exactos (no "legibles")
```
Cada imagen se construye por **capas**: cada instrucción `RUN`, `COPY`, `ADD`... del Dockerfile genera una capa nueva. `docker history` las lista de la más reciente (arriba) a la base (abajo).

### Vía 1 (recomendada): Dockerfile + `docker build`

Un **Dockerfile** es un fichero de texto con las instrucciones para construir la imagen. Ejemplo:
```dockerfile
# Dockerfile
FROM nginx:latest                     # imagen base de la que partimos
LABEL maintainer="jose@ejemplo.com"   # metadatos (opcional)
COPY ./sitio /usr/share/nginx/html    # copia archivos del host a la imagen
RUN apt-get update && apt-get install -y curl   # ejecuta comandos al construir
EXPOSE 80                             # documenta el puerto que usa
ENV ENTORNO=produccion                # variable de entorno
CMD ["nginx", "-g", "daemon off;"]    # comando que se ejecuta al arrancar
```

Instrucciones más usadas del Dockerfile:

| Instrucción | Para qué sirve |
|---|---|
| `FROM` | Imagen base de la que se parte (obligatoria, va primera) |
| `COPY` / `ADD` | Copiar archivos del host a la imagen (`ADD` además descomprime/URLs) |
| `RUN` | Ejecutar un comando **durante la construcción** (instalar paquetes...) |
| `CMD` | Comando por defecto **al arrancar** el contenedor (se puede sobrescribir) |
| `ENTRYPOINT` | Comando fijo al arrancar (los argumentos de `run` se le añaden) |
| `WORKDIR` | Directorio de trabajo dentro de la imagen |
| `EXPOSE` | Documenta el puerto que usa el contenedor (no lo publica) |
| `ENV` | Define variables de entorno |
| `VOLUME` | Declara un punto de montaje para datos persistentes |

#### Variables: `ENV` vs `ARG`

Las dos definen variables en el Dockerfile, pero en momentos distintos:

| | `ARG` | `ENV` |
|---|---|---|
| ¿Cuándo existe? | Solo **durante la construcción** (`build`) | Durante la construcción **y** dentro del contenedor en ejecución |
| ¿Se puede cambiar al construir? | Sí, con `--build-arg` | No directamente (pero un `ENV` puede tomar su valor de un `ARG`) |
| ¿Visible con `docker exec env`? | ❌ No | ✅ Sí |
| Uso típico | Versión a instalar, rutas de build, flags temporales | Configuración del programa en marcha (puerto, modo, idioma...) |

```dockerfile
# ARG: parámetro solo de construcción
ARG VERSION=1.25                 # valor por defecto, se puede sobrescribir
FROM nginx:${VERSION}

# ENV: variable que queda en el contenedor
ENV ENTORNO=produccion
ENV TZ=Europe/Madrid

# Un ENV puede heredar el valor de un ARG
ARG APP_PORT=8080
ENV PORT=${APP_PORT}
```

Sobrescribir los valores:
```bash
# ARG: se cambia al CONSTRUIR
docker build --build-arg VERSION=1.27 -t web .

# ENV: se cambia al EJECUTAR
docker run -e ENTORNO=desarrollo web
```
> [!warning]
> No pongas **secretos** (contraseñas, tokens) en `ARG` ni `ENV`: quedan registrados en la imagen y se ven con `docker history` o `docker inspect`.

#### `docker build`
Construye una imagen a partir de un Dockerfile.
```bash
docker build -t miusuario/mi_imagen:1.0 .    # -t: nombre:etiqueta ; "." = carpeta actual
docker build -t web:dev -f Dockerfile.dev .  # -f: usar otro Dockerfile
docker build --no-cache -t web .             # construir sin usar caché
```
- El punto final (`.`) es el **contexto de construcción**: la carpeta que se envía a Docker (donde suele estar el Dockerfile).
- Conviene un fichero **`.dockerignore`** (como `.gitignore`) para no enviar archivos innecesarios al contexto.

### Vía 2 (rápida): `docker commit`

Crea una imagen a partir de un contenedor **existente**, congelando su estado actual. Útil para pruebas, pero **no deja constancia** de cómo se hizo (al contrario que el Dockerfile).
```bash
docker commit web mi_imagen:snapshot
docker commit -m "con curl instalado" -a "Jose" web mi_imagen:1.0
```

### Publicar la imagen (opcional)

#### `docker login` / `docker push`
Sube tu imagen a un registro (Docker Hub). El nombre debe incluir tu usuario.
```bash
docker login                               # autenticarse en Docker Hub
docker push miusuario/mi_imagen:1.0        # subir la imagen
```

> [!tip] Flujo típico
> 1. Escribir el `Dockerfile` → 2. `docker build -t nombre:tag .` → 3. `docker run` para probarla → 4. (opcional) `docker tag` + `docker push` para publicarla.

---

## 💾 Persistencia de datos: volúmenes y bind mounts

Cuando borras un contenedor, **pierdes lo que había dentro**. Para que los datos persistan (o para compartir archivos con el host) se montan almacenamientos externos. Hay tres tipos:

| Tipo | Qué es | Lo gestiona | Uso típico |
|---|---|---|---|
| **Volumen** | Almacén gestionado por Docker (en `/var/lib/docker/volumes/`) | Docker | Datos de producción (BBDD, etc.) |
| **Bind mount** | Una carpeta **concreta del host** montada en el contenedor | Tú (ruta del host) | Desarrollo: editar código del host y verlo dentro |
| **tmpfs** | Almacenamiento en **memoria RAM** (no persiste) | Docker | Datos temporales/sensibles |

### Volúmenes (gestionados por Docker)

Comandos de gestión:
```bash
docker volume create datos           # crear un volumen con nombre
docker volume ls                     # listar volúmenes
docker volume inspect datos          # ver detalles (ruta real, etc.)
docker volume rm datos               # borrar un volumen
docker volume prune                  # borrar todos los volúmenes sin usar
```

Montarlo en un contenedor (con `-v nombre:/ruta/en/contenedor`):
```bash
docker run -d --name db -v datos:/var/lib/mysql mysql
```
- Si el volumen `datos` no existe, Docker **lo crea solo**.
- Aunque borres el contenedor `db`, el volumen `datos` (y la BBDD) **sigue existiendo**.

### Bind mounts (carpeta del host)

Monta una **ruta concreta de tu máquina** dentro del contenedor. La sintaxis es `-v /ruta/host:/ruta/contenedor` (la del host debe ser **absoluta**).
```bash
docker run -d --name web -v /home/jose/sitio:/usr/share/nginx/html nginx
docker run -d -v "$(pwd)":/app node        # montar la carpeta actual
docker run -d -v /home/jose/conf:/etc/nginx:ro nginx   # ro = solo lectura
```
- Lo que cambies en el host se ve **al instante** dentro del contenedor y viceversa (ideal para desarrollar).
- `:ro` lo monta en **solo lectura**; por defecto es lectura/escritura (`:rw`).

### Sintaxis alternativa `--mount` (más explícita)
Equivalente a `-v` pero más legible; la recomienda Docker para scripts.
```bash
docker run -d --mount type=volume,source=datos,target=/var/lib/mysql mysql
docker run -d --mount type=bind,source=/home/jose/sitio,target=/usr/share/nginx/html nginx
docker run -d --mount type=tmpfs,target=/tmp/cache nginx
```

### Volumen vs bind mount: ¿cuál uso?
- **Volumen** → cuando solo te importa que los datos **persistan** y los gestione Docker (bases de datos, uploads...). Más portable.
- **Bind mount** → cuando necesitas **compartir una carpeta concreta** del host (código fuente en desarrollo, ficheros de configuración).

> [!tip]
> Comprueba qué tiene montado un contenedor con `docker inspect -f '{{json .Mounts}}' nombre`.

---

## 🌐 Redes y puertos

### Tipos de red (drivers)

| Driver | Qué hace | Uso típico |
|---|---|---|
| **bridge** | Red privada virtual en el host (la **por defecto**). Los contenedores se comunican entre sí y salen a internet por NAT | Lo habitual en una sola máquina |
| **host** | El contenedor usa **directamente** la red del host (sin aislamiento ni NAT) | Máximo rendimiento, sin mapear puertos |
| **none** | Sin red ninguna | Contenedores aislados |
| **overlay** | Red que conecta contenedores en **varias máquinas** (Swarm) | Clústeres multi-host |
| **macvlan** | Da al contenedor una **MAC/IP propia** en la red física | Que aparezca como un equipo más de la LAN |

```bash
docker run -d --network host nginx     # usar la red del host
docker run -d --network none alpine    # sin red
```

> [!note] bridge por defecto vs bridge personalizada
> Todos los contenedores caen en la red `bridge` por defecto, pero en ella **no** se resuelven por nombre. Si creas tu **propia** red bridge, los contenedores se encuentran entre sí **por su nombre** (DNS interno). Por eso se recomienda crear redes personalizadas.

### Gestión de redes (`docker network`)
```bash
docker network ls                      # listar redes
docker network inspect bridge          # ver detalles (contenedores, IPs...)
docker network create mi_red           # crear una red bridge personalizada
docker network create --driver bridge --subnet 172.20.0.0/16 mi_red   # con subred
docker network rm mi_red               # borrar una red
docker network prune                   # borrar todas las redes sin usar
```

### Conectar / desconectar contenedores de una red
```bash
docker run -d --name web --network mi_red nginx   # arrancar YA en una red
docker network connect mi_red web                 # añadir a una red (en caliente)
docker network disconnect mi_red web              # quitar de una red
```
Un contenedor puede estar conectado a **varias redes** a la vez.

### Comunicación entre contenedores por nombre
Si dos contenedores están en la **misma red personalizada**, se llaman por su **nombre** (no hace falta saber la IP):
```bash
docker network create app_red
docker run -d --name db --network app_red mysql
docker run -d --name web --network app_red nginx
# Desde 'web' puedes hacer:  ping db   /  conectar a  db:3306
```

### Publicar puertos (del host al contenedor)

Por defecto, el puerto de un contenedor **no es accesible** desde fuera del host. Hay que **publicarlo** con `-p HOST:CONTENEDOR`.
```bash
docker run -d -p 8080:80 nginx          # localhost:8080 -> puerto 80 del contenedor
docker run -d -p 80:80 -p 443:443 nginx # varios puertos
docker run -d -p 127.0.0.1:8080:80 nginx# solo accesible desde la propia máquina
docker run -d -p 8080:80/udp dns        # especificar protocolo UDP
docker run -d -P nginx                  # -P (mayús): publica TODOS los EXPOSE en puertos al azar
```
- Izquierda = puerto del **host**; derecha = puerto del **contenedor**.
- `docker port web` muestra qué puertos tiene publicados un contenedor.

### Puertos desde el Dockerfile: `EXPOSE`
```dockerfile
EXPOSE 80          # documenta que el contenedor "escucha" en el 80
EXPOSE 80 443      # varios
EXPOSE 53/udp      # con protocolo
```
> [!important] `EXPOSE` NO publica el puerto
> `EXPOSE` es solo **documentación**: indica en qué puerto escucha el contenedor, pero **no lo abre** al host. Para acceder desde fuera sigue haciendo falta `-p` (o `-P`, que sí usa los `EXPOSE` para publicarlos automáticamente en puertos aleatorios).

### Contenedores "a la escucha"
Un contenedor "escucha" en un puerto si el proceso de dentro tiene un servicio abierto ahí (nginx en el 80, mysql en el 3306...). Para comprobarlo:
```bash
docker port web                         # puertos publicados hacia el host
docker exec web ss -tlnp                # puertos en escucha DENTRO del contenedor
docker exec web netstat -tlnp           # alternativa (si tiene net-tools)
```
Resumen de capas: el **proceso** escucha en un puerto → `EXPOSE` lo documenta → `-p` lo publica al host → el cliente accede por `HOST:PUERTO`.

---

## 🧩 Docker Compose

**Docker Compose** permite definir y ejecutar aplicaciones con **varios contenedores** a la vez, describiéndolo todo (imágenes, puertos, volúmenes, redes, variables...) en un único fichero **`compose.yaml`** (o el clásico `docker-compose.yml`). En vez de teclear comandos `docker run` largos, lo arrancas todo con un comando.

> [!note] `docker compose` (v2) vs `docker-compose` (v1)
> En las versiones actuales es un subcomando de Docker: `docker compose` (con espacio). El antiguo `docker-compose` (con guion) es la v1, ya en desuso.

### Fichero de ejemplo (`compose.yaml`)
```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"                # igual que -p
    volumes:
      - ./sitio:/usr/share/nginx/html   # bind mount
    networks:
      - app_red
    depends_on:
      - db                       # arranca 'db' antes que 'web'

  db:
    image: mysql:8
    environment:                 # igual que -e
      MYSQL_ROOT_PASSWORD: secreto
      MYSQL_DATABASE: miapp
    volumes:
      - datos_db:/var/lib/mysql  # volumen con nombre
    networks:
      - app_red
    restart: always              # política de reinicio

volumes:
  datos_db:                      # declara el volumen gestionado por Docker

networks:
  app_red:                       # red personalizada (los servicios se ven por nombre)
```
- Cada entrada bajo `services:` es un contenedor. Se llaman entre sí **por el nombre del servicio** (`web` puede conectar a `db`).
- Puedes usar `build: .` en vez de `image:` para construir desde un Dockerfile.

### Comandos principales (`docker compose`)
Se ejecutan en la carpeta donde está el `compose.yaml`.
```bash
docker compose up               # crea y arranca todo (en primer plano)
docker compose up -d            # en segundo plano (detached)
docker compose up -d --build    # reconstruye las imágenes antes de arrancar
docker compose down             # para y ELIMINA contenedores y redes
docker compose down -v          # además borra los volúmenes
docker compose stop             # para sin borrar
docker compose start            # vuelve a arrancar los parados
docker compose ps               # estado de los servicios
docker compose logs -f          # logs de todos los servicios en directo
docker compose logs -f web      # logs de un servicio concreto
docker compose exec web bash    # entrar en el contenedor del servicio 'web'
docker compose build            # solo construir las imágenes
docker compose pull             # descargar las imágenes
docker compose restart          # reiniciar los servicios
docker compose config           # validar y mostrar el fichero ya resuelto
```

> [!tip] La gran ventaja
> Si cambias algo en el `compose.yaml` (un puerto, una variable...), basta con `docker compose up -d`: Compose detecta qué contenedores han cambiado y **solo recrea esos**, dejando el resto intactos.

> [!warning] `down` borra contenedores
> `docker compose down` **elimina** los contenedores y la red (como un `rm`). Los datos se salvan si están en **volúmenes**; con `down -v` se borran también los volúmenes. Para solo apagar sin borrar, usa `stop`.

### Variables de entorno y `.env`
Compose lee automáticamente un fichero **`.env`** en la misma carpeta:
```bash
# .env
MYSQL_PASSWORD=secreto
TAG=8
```
```yaml
services:
  db:
    image: mysql:${TAG}                 # usa la variable del .env
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_PASSWORD}
```


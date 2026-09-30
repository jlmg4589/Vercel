---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-5-configuracion-de-dispositivos-y-sistemas/practicas/p4-ud-5-proxy-cache-con-squid/","title":"Proxy caché con Squid","tags":["bastionado-de-redes-y-sistemas","practica","proxy"],"noteIcon":"","dg-note-properties":{"title":"Proxy caché con Squid","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","proxy"],"status":"En progreso"}}
---


# 📝 Práctica: Proxy caché con Squid

> [!note] Objetivos de la Práctica
> - Instalar y configurar Squid como proxy-caché para una red local.
> - Controlar el acceso web con ACL por red, horario y URL.
> - Analizar los *logs* del proxy.

## 📐 Recordatorio Teórico

- Concepto, funciones, ventajas e inconvenientes de los proxies: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/10. Configuración segura de cortafuegos, enrutadores y proxies#10.6 Servidores proxy\|10.6]].
- Parámetros, ACL y `http_access` de Squid: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/10. Configuración segura de cortafuegos, enrutadores y proxies#10.6.4 Squid\|10.6.4]].
- Logs de Squid: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/8. Herramientas de almacenamiento de logs#8.5.2 Logs del proxy Squid\|8.5.2]].

**Entorno:** servidor Ubuntu con dos interfaces (Internet y red local 192.168.1.0/24) y un cliente en la red local.

## 🛠️ Enunciado

1. Instala Squid y haz copia de la configuración original:
   ```bash
   sudo apt-get install squid
   sudo cp /etc/squid/squid.conf /etc/squid/squid.conf.orig
   ```
2. Configura los parámetros generales: puerto 3128 asociado a la IP interna, 256 MB de caché en RAM, caché en disco de 512 MB (16 × 256), tamaño máximo de objeto 4096 KB y nombre visible `servidor`:
   ```text
   http_port 192.168.1.1:3128
   cache_mem 256 MB
   cache_dir ufs /var/spool/squid 512 16 256
   maximum_object_size 4096 KB
   visible_hostname servidor
   ```
3. **Ejemplo 1 · Salida sin restricciones** para la red local. En la zona «INSERT YOUR OWN RULE(S) HERE» o en `include /etc/squid/misreglas.conf`:
   ```text
   acl todo src all
   acl localhost src 127.0.0.1/255.255.255.255
   acl redlocal src 192.168.1.0/255.255.255.0
   http_access allow localhost
   http_access allow redlocal
   http_access deny todo
   ```
4. Crea los directorios de caché, comprueba la configuración y arranca:
   ```bash
   sudo squid -z
   sudo squid -k parse
   sudo service squid start
   ```
5. En el navegador del cliente configura el proxy HTTP (IP 192.168.1.1, puerto 3128) y comprueba la navegación.
6. **Ejemplo 2 · Restricción de acceso web**: crea `/etc/squid/sitios_denegados` con dominios y palabras (una por línea) y deniégalos **antes** de cualquier `allow`:
   ```text
   acl denegados url_regex "/etc/squid/sitios_denegados"
   http_access deny denegados
   http_access allow localhost
   http_access allow redlocal
   http_access deny todo
   ```
   Recarga con `sudo service squid reload` y verifica el bloqueo.
7. **Horario**: permite la navegación de la red local solo de lunes a viernes de 9:00 a 17:00 (`acl horario time MTWHF 9:00-17:00` y `http_access allow redlocal horario`).
8. **Exclusiones**: crea el fichero `/etc/squid/no_permitidos` con la IP de un equipo concreto y niégale el acceso con `http_access allow redlocal !no_permitidos`.
9. **Límite de conexiones**: define `acl maxcon maxconn 10` y deniega a la red local cuando lo supere (`http_access deny redlocal maxcon`).
10. **Análisis de logs**: revisa `/var/log/squid/access.log`, `store.log` y `cache.log`:
    - Identifica peticiones denegadas y el cliente que las hizo.
    - Identifica peticiones servidas desde caché.
    - Explica qué información guarda cada fichero.

> [!info] Contenido complementario (fuente externa)
> **Proxy transparente (ampliación):** Squid escucha en un puerto con el modo `intercept` (p. ej. `http_port 3129 intercept`) y la puerta de enlace redirige el tráfico web a ese puerto con una regla `REDIRECT`/`DNAT` del cortafuegos. Ver [Linux traffic Interception using REDIRECT – Squid Wiki](https://wiki.squid-cache.org/ConfigExamples/Intercept/LinuxRedirect) y [directiva http_port](https://www.squid-cache.org/Doc/config/http_port/). El significado de los códigos del `access.log` (TCP_HIT, TCP_MISS, TCP_DENIED…) queda pendiente de fuente.

## ✅ Criterios de evaluación asociados

- **RA5 a)** Se han configurado dispositivos de seguridad perimetral acorde a una serie de requisitos de seguridad.
- **RA5 c)** Se han identificado comportamientos no deseados en una red a través del análisis de los registros (*logs*).

> [!quote]- Fuentes
> - `00-Unidad 3 - Bastionado de redes de area local.odt` (Servidores proxy, Squid: parámetros, logs, ACL, http_access, ejemplos 1 y 2)
> - `UT 5-Configuración de Dispositivos de SI.pdf` (apdo. 9.2 proxies)

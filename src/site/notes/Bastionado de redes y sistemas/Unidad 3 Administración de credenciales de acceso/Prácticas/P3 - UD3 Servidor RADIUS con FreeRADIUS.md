---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-3-administracion-de-credenciales-de-acceso/practicas/p3-ud-3-servidor-radius-con-free-radius/","title":"Servidor RADIUS con FreeRADIUS","tags":["bastionado-de-redes-y-sistemas","practica","radius"],"noteIcon":"","dg-note-properties":{"title":"Servidor RADIUS con FreeRADIUS","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","radius"],"status":"En progreso"}}
---


# 📝 Práctica: Servidor RADIUS con FreeRADIUS

> [!note] Objetivos de la Práctica
> - Instalar un servidor RADIUS (FreeRADIUS) en Linux.
> - Dar de alta un cliente NAS y usuarios.
> - Comprobar la autenticación centralizada y aplicar medidas de seguridad.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 3 Administración de credenciales de acceso/Apuntes/7. RADIUS, TACACS y Kerberos#7.2 RADIUS\|RADIUS]]
- [[Bastionado de redes y sistemas/Unidad 3 Administración de credenciales de acceso/Apuntes/7. RADIUS, TACACS y Kerberos#7.2.5 Seguridad en RADIUS\|Seguridad en RADIUS]]
- [[Bastionado de redes y sistemas/Unidad 3 Administración de credenciales de acceso/Apuntes/5. Gestión de accesos. Sistemas NAC\|5. Gestión de accesos. Sistemas NAC]]

**Escenario de la fuente:** un portátil se conecta a la wifi de un punto de acceso (NAS), que envía las credenciales al servidor RADIUS; si son válidas, el punto de acceso entrega la configuración IP por DHCP.

> [!info] Contenido complementario (fuente externa)
> Las fuentes del curso solo remiten a enlaces y vídeos ("FreeRadius: WiFi más seguro", instalación en Linux y en Windows Server 2012). Los pasos siguientes siguen la guía oficial de FreeRADIUS v3: prueba en modo depuración (`-X`), usuario en `mods-config/files/authorize`, `radtest` y alta de clientes en `clients.conf`. En Debian/Ubuntu el demonio se llama `freeradius` y la configuración está en `/etc/freeradius/3.0/` (en otras distribuciones, `radiusd` y `/etc/raddb/`) (actualizado, fuente: [FreeRADIUS Wiki – Getting Started](https://wiki.freeradius.org/guide/Getting-Started)).

## 🛠️ Enunciado

### Paso 1. Instalación

```bash
sudo apt update
sudo apt install freeradius freeradius-utils
```

### Paso 2. Alta del cliente NAS

En `/etc/freeradius/3.0/clients.conf` añade el punto de acceso (o la máquina de pruebas) con un **secreto compartido robusto**:

```text
client punto_acceso {
    ipaddr = 192.168.1.10
    secret = UnSecretoLargoYAleatorio_2026
}
```

### Paso 3. Alta de usuarios

En `/etc/freeradius/3.0/mods-config/files/authorize`:

```text
alumno1 Cleartext-Password := "P4ssw0rd!Segura"
```

### Paso 4. Arranque en modo depuración y prueba

```bash
sudo systemctl stop freeradius
sudo freeradius -X
# En otra terminal:
radtest alumno1 'P4ssw0rd!Segura' 127.0.0.1 0 testing123
```

Comprueba la respuesta con credenciales correctas e incorrectas.

### Paso 5. Integración con el punto de acceso (opcional)

Configura un punto de acceso en modo WPA2-Enterprise apuntando al servidor RADIUS (puerto 1812/UDP) y conéctate desde un cliente.

### Paso 6. Cuestiones de seguridad

1. Enumera las debilidades de RADIUS vistas en teoría y las contramedidas aplicables (IPSec, CHAP/EAP, DIAMETER).
2. ¿Qué información registra la contabilidad (*accounting*) y dónde puede almacenarse?

> [!important] Criterio de evaluación asociado
> RA3 e) Se ha instalado y configurado un servidor seguro para la administración de credenciales (tipo RADIUS).

> [!quote]- Fuentes
> - `Bloque 3- Admon Credenciales Acceso S.Infomáticos.pdf` (apdo. 6.2: escenario wifi, FreeRADIUS, debilidades)
> - `2-RADIUS.pdf`; `00-Unidad 1 - Mecanismos de autenticación y gestión de credenciales.odt` (servidor RADIUS)

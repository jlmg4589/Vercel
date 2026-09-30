---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-2-control-de-acceso-y-autenticacion/practicas/p1-ud-2-gestion-segura-de-usuarios-grupos-y-contrasenas/","title":"Gestión segura de usuarios, grupos y contraseñas","tags":["bastionado-de-redes-y-sistemas","practica","control-de-acceso"],"noteIcon":"","dg-note-properties":{"title":"Gestión segura de usuarios, grupos y contraseñas","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","control-de-acceso"],"status":"En progreso"}}
---


# 📝 Práctica: Gestión segura de usuarios, grupos y contraseñas

> [!note] Objetivos de la Práctica
> - Conocer los ficheros `/etc/passwd`, `/etc/shadow` y `/etc/group`.
> - Crear y administrar usuarios y grupos aplicando el principio de mínimo privilegio.
> - Configurar la caducidad de contraseñas con `login.defs` y `chage`.
> - Auditar el sistema en busca de cuentas inseguras.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Apuntes/5. Control de acceso#5.8.1 Cuentas de usuario\|Cuentas de usuario]]
- [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Apuntes/5. Control de acceso#5.8.2 Ficheros de usuarios y contraseñas en Linux\|Ficheros de usuarios y contraseñas en Linux]]
- [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Apuntes/5. Control de acceso#5.8.4 Política de contraseñas y comprobaciones generales\|Política de contraseñas]]
- [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Apuntes/3. Mecanismos y factores de autenticación#3.3.5 Políticas de contraseñas\|Políticas de contraseñas]]

## 🛠️ Enunciado

Trabaja en una máquina virtual Linux (Debian/Ubuntu) con un usuario con permisos de `sudo`.

1. **Analiza los ficheros de usuarios.** Muestra tu entrada en cada fichero e identifica cada campo (algoritmo de *hash*, *salt*, días de validez…).

   ```bash
   getent passwd $USER
   sudo getent shadow $USER
   getent group sudo
   ```

2. **Configura los valores por defecto** en `/etc/login.defs` para los nuevos usuarios:

   ```bash
   PASS_MAX_DAYS 60
   PASS_MIN_DAYS 7
   PASS_WARN_AGE 7
   UMASK 077
   ENCRYPT_METHOD SHA512
   ```

3. **Crea un grupo y dos usuarios** con *home* y *shell*:

   ```bash
   sudo groupadd alumnos
   sudo useradd -m -g alumnos -s /bin/bash juan
   sudo useradd -m -g alumnos -s /bin/bash maria
   sudo passwd juan
   sudo passwd maria
   ```

   Comprueba los permisos del *home* creado (`ls -ld /home/juan`) y relaciónalos con `UMASK`.

4. **Ajusta la caducidad** de `juan` y compruébala:

   ```bash
   sudo chage -M 30 -m 1 -W 5 -I 7 juan
   sudo chage -l juan
   ```

5. **Bloquea y desbloquea** la contraseña de `maria` y observa el `!` en `/etc/shadow`:

   ```bash
   sudo passwd -l maria
   sudo grep maria /etc/shadow
   sudo passwd -u maria
   ```

6. **Crea un usuario de servicio sin shell** y comprueba que no puede iniciar sesión:

   ```bash
   sudo useradd -r -s /usr/sbin/nologin servicioweb
   sudo su - servicioweb
   ```

7. **Audita el sistema:**

   ```bash
   sudo awk -F: '($2 == "") {print}' /etc/shadow   # cuentas sin contraseña
   awk -F: '($2 != "x") {print}' /etc/passwd       # hashes fuera de shadow
   awk -F: '($3 == "0") {print}' /etc/passwd       # cuentas con UID 0
   awk -F: '($7 !~ /nologin|false/) {print $1, $7}' /etc/passwd   # cuentas con shell
   ```

   Indica qué cuentas con *shell* serían innecesarias en un servidor bastionado.

8. **Configura `sudo`** para que `juan` solo pueda reiniciar el servicio SSH (`sudo visudo`):

   ```text
   juan ALL=(root) /usr/bin/systemctl restart ssh
   ```

9. **Limpieza:** elimina a `maria` con su *home* (`sudo userdel -r maria`).

## ✅ Entrega

Documento con capturas de cada paso y respuesta razonada a las preguntas planteadas.

**Criterio de evaluación asociado:** RA2 b) (políticas de autenticación basadas en contraseñas).

> [!quote]- Fuentes
> - `C11+-+S4_01+-+ficheros+usuarios,+grupros+y+claves-1.pdf`, `C11+-+S4_02+-+usuarios,+grupos,+permisos.pdf`, `C11+-+S4_06-Seguridad.pdf`, `C11+-+S4_07+-+opconies+usuario+y+pam_tally2.pdf`
> - `Bloque 2- Sistemas Control de Acceso.pdf` (apdo. 4.7)

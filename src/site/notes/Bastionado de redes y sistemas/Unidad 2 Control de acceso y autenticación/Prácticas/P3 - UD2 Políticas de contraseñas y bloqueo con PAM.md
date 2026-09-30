---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-2-control-de-acceso-y-autenticacion/practicas/p3-ud-2-politicas-de-contrasenas-y-bloqueo-con-pam/","title":"Políticas de contraseñas y bloqueo con PAM","tags":["bastionado-de-redes-y-sistemas","practica","pam"],"noteIcon":"","dg-note-properties":{"title":"Políticas de contraseñas y bloqueo con PAM","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","pam"],"status":"En progreso"}}
---


# 📝 Práctica: Políticas de contraseñas y bloqueo con PAM

> [!note] Objetivos de la Práctica
> - Entender la estructura de los ficheros de `/etc/pam.d/`.
> - Imponer complejidad de contraseñas con `pam_pwquality`.
> - Evitar la reutilización de contraseñas.
> - Bloquear cuentas tras varios intentos fallidos.
> - Endurecer el acceso SSH.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Apuntes/7. Pluggable Authentication Modules (PAM)\|7. Pluggable Authentication Modules (PAM)]]
- [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Apuntes/3. Mecanismos y factores de autenticación#3.2 Ataques a la autenticación\|Ataques a la autenticación]]
- [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Apuntes/5. Control de acceso#5.7.2 Configuración del acceso SSH\|Configuración del acceso SSH]]

> [!warning] Antes de empezar
> Haz una **instantánea** de la máquina virtual y mantén **una sesión de root abierta** mientras editas PAM: un error puede impedir iniciar sesión.

## 🛠️ Enunciado

1. **Explora PAM:** lista `/etc/pam.d/` y explica, para cada línea de `common-auth` y `common-password`, su tipo, control y módulo.

2. **Complejidad con `pam_pwquality`:**

   ```bash
   sudo apt install libpam-pwquality
   ```

   Edita `/etc/security/pwquality.conf`:

   ```text
   minlen = 12
   dcredit = -1
   ucredit = -1
   lcredit = -1
   ocredit = -1
   difok = 4
   maxrepeat = 2
   badwords = instituto empresa
   ```

   Comprueba la robustez de varias contraseñas y prueba a cambiar la de `juan` con contraseñas que no cumplan:

   ```bash
   echo "Contraseña" | pwscore
   sudo -u juan passwd
   ```

3. **Histórico de contraseñas:** en `/etc/pam.d/common-password`, añade `remember=5` a la línea de `pam_unix.so` y comprueba que no se puede reutilizar una contraseña reciente.

4. **Bloqueo por intentos fallidos** (`pam_tally2`, según el tutorial) en `/etc/pam.d/common-auth`:

   ```text
   auth required pam_tally2.so deny=4 unlock_time=900 even_deny_root
   ```

   Falla 4 veces el inicio de sesión de `juan` en una consola y consulta/restablece el contador:

   ```bash
   sudo pam_tally2 -u juan
   sudo pam_tally2 -u juan --reset
   ```

   > [!note]
   > Si tu distribución ya no incluye `pam_tally2`, usa su sustituto `pam_faillock` (`deny=4 unlock_time=900`) y el comando `faillock --user juan [--reset]` (conocimiento general, verificar).

5. **Tiempo de inactividad:** añade a `/etc/profile` `TMOUT=120` y `readonly TMOUT` y comprueba que la sesión se cierra.

6. **Endurece SSH** en `/etc/ssh/sshd_config` (`PermitRootLogin no`, `PermitEmptyPasswords no`, `AllowUsers juan`, `ClientAliveInterval 300`, `ClientAliveCountMax 0`, `Banner /etc/issue`), configura el acceso con clave pública y, tras comprobarlo, desactiva `PasswordAuthentication`:

   ```bash
   ssh-keygen -t rsa -b 4096
   ssh-copy-id juan@servidor
   ssh juan@servidor
   sudo systemctl restart ssh
   ```

7. **Auditoría:** usa **John the Ripper** sobre una copia de `/etc/shadow` de la MV de pruebas con una contraseña débil y otra que cumpla la política; compara resultados.

   ```bash
   sudo unshadow /etc/passwd /etc/shadow > hashes.txt
   john hashes.txt
   ```

## ✅ Entrega

Documento con capturas, las líneas de configuración añadidas y una conclusión sobre qué ataques mitiga cada medida.

**Criterios de evaluación asociados:** RA2 b) (contraseñas y frases de paso) y c) (acceso por clave pública/certificado en SSH).

> [!quote]- Fuentes
> - `C11 - S4_07 opciones usuario y pam_tally2`
> - `Bloque 2- Sistemas Control de Acceso.pdf` (apdos. 4.6 y 4.7.2)

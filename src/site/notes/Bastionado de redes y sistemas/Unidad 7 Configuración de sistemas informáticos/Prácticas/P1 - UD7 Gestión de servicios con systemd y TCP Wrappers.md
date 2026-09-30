---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-7-configuracion-de-sistemas-informaticos/practicas/p1-ud-7-gestion-de-servicios-con-systemd-y-tcp-wrappers/","title":"Gestión de servicios con systemd y TCP Wrappers","tags":["bastionado-de-redes-y-sistemas","practica","reduccion-de-servicios"],"noteIcon":"","dg-note-properties":{"title":"Gestión de servicios con systemd y TCP Wrappers","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","reduccion-de-servicios"],"status":"En progreso"}}
---


# 📝 Práctica: Gestión de servicios con systemd y TCP Wrappers

> [!note] Objetivos de la Práctica
> - Enumerar los servicios instalados y los que escuchan en red.
> - Deshabilitar y enmascarar servicios innecesarios.
> - Restringir el acceso a servicios con TCP Wrappers y xinetd.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/2. Reducción del número de servicios\|2. Reducción del número de servicios]]
- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/4. Eliminación de protocolos de red innecesarios\|4. Eliminación de protocolos de red innecesarios]]

## 🛠️ Enunciado

Entorno: una máquina virtual Debian/Ubuntu y otra máquina cliente en la misma red.

1. Lista los servicios cargados, los habilitados en el arranque y los que escuchan en red:

   ```bash
   systemctl
   systemctl list-unit-files --type service
   systemctl list-unit-files | grep enabled
   ss -tulpn
   ```

2. Elige el servicio `cron` y prueba el ciclo completo; anota la diferencia entre `restart` y `reload`, y entre `stop` y `disable`:

   ```bash
   systemctl status cron
   sudo systemctl stop cron
   sudo systemctl start cron
   sudo systemctl reload cron
   systemctl is-enabled cron
   ```

3. Deshabilita un servicio innecesario (p. ej. `bluetooth.service`) y comprueba que un administrador aún puede iniciarlo a mano. Después enmascáralo y verifica que ya no puede iniciarse:

   ```bash
   sudo systemctl disable bluetooth.service
   sudo systemctl start bluetooth.service
   sudo systemctl mask bluetooth.service
   sudo systemctl start bluetooth.service   # debe fallar
   sudo systemctl unmask bluetooth.service
   ```

4. Comprueba si `sshd` está compilado con soporte de TCP Wrappers:

   ```bash
   strings /usr/sbin/sshd | grep hosts_access
   ldd /usr/sbin/sshd | grep libwrap
   ```

5. Si lo está, permite SSH solo desde tu red y deniega el resto. Prueba desde un equipo autorizado y otro no autorizado:

   ```text
   # /etc/hosts.allow
   sshd: 192.168.1.
   # /etc/hosts.deny
   ALL: ALL
   ```

6. Crea `/etc/nologin` con un mensaje de mantenimiento, intenta entrar con un usuario normal y después bórralo.

7. (Opcional) Instala `xinetd` y define un servicio con `only_from`, `access_times` e `instances`, tomando como modelo el ejemplo de los apuntes.

8. Entrega un informe con la lista de servicios eliminados y el motivo de cada uno.

**Criterio de evaluación asociado:** RA7 a) Se han enumerado y eliminado los programas, servicios y protocolos innecesarios que hayan sido instalados por defecto en el sistema.

---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-7-configuracion-de-sistemas-informaticos/practicas/p2-ud-7-securizacion-del-servidor-ssh/","title":"Securización del servidor SSH","tags":["bastionado-de-redes-y-sistemas","practica","administracion-remota"],"noteIcon":"","dg-note-properties":{"title":"Securización del servidor SSH","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","administracion-remota"],"status":"En progreso"}}
---


# 📝 Práctica: Securización del servidor SSH

> [!note] Objetivos de la Práctica
> - Endurecer la configuración de `sshd`.
> - Configurar la autenticación por clave pública.
> - Auditar el servidor con `ssh-audit`.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/5. Securización de la administración remota#5.4.1 Securizar SSH\|Securizar SSH]]
- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/2. Reducción del número de servicios\|2. Reducción del número de servicios]] (Telnet y rsh frente a SSH)

## 🛠️ Enunciado

Entorno: servidor Debian/Ubuntu con `openssh-server` y un cliente Linux.

1. Audita la configuración inicial y guarda el resultado:

   ```bash
   ssh-audit ip_servidor > audit-antes.txt
   ```

2. En el cliente, genera un par de claves y copia la pública al servidor:

   ```bash
   ssh-keygen -t rsa -b 4096
   ssh-copy-id -i ~/.ssh/id_rsa.pub usuario@ip_servidor
   ssh usuario@ip_servidor        # debe entrar sin contraseña de usuario
   ```

   > [!info] Actualización 2026
   > Con OpenSSH actual se recomienda `ssh-keygen -t ed25519` (tipo por defecto desde OpenSSH 9.5) y no hace falta la directiva `Protocol 2` (SSH-1 eliminado en OpenSSH 7.6). Fuentes: [OpenSSH 9.5](https://www.openssh.com/txt/release-9.5), [OpenSSH 7.6](https://www.openssh.com/txt/release-7.6).

3. Comprueba los puertos en uso y elige uno libre (p. ej. 5022):

   ```bash
   ss -ntap
   ```

4. Edita `/etc/ssh/sshd_config`:

   ```text
   Port 5022
   PermitRootLogin no
   AllowUsers usuario
   MaxAuthTries 3
   PasswordAuthentication no
   AuthorizedKeysFile .ssh/authorized_keys
   X11Forwarding no
   AllowTcpForwarding no
   AllowStreamLocalForwarding no
   GatewayPorts no
   PermitTunnel no
   PrintLastLog no
   PrintMotd no
   ```

5. Reinicia el servicio **sin cerrar la sesión actual** y prueba desde otra terminal:

   ```bash
   sudo systemctl restart ssh
   ssh -p 5022 usuario@ip_servidor
   ssh -p 5022 root@ip_servidor     # debe ser rechazado
   ```

6. Vuelve a auditar y compara:

   ```bash
   ssh-audit -p 5022 ip_servidor > audit-despues.txt
   ```

7. (Opcional) Instala Fail2ban o DenyHosts y comprueba que bloquea una IP tras varios intentos fallidos.

8. (Opcional) Con el *tunneling* aún permitido en otro servidor de pruebas, crea un túnel `ssh -N -f -L 8080:destino:80 usuario@origen` y explica por qué se desactiva en producción.

**Criterio de evaluación asociado:** RA7 c) Se ha incrementado la seguridad del sistema de administración remoto SSH y otros.

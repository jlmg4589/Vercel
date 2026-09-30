---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-7-configuracion-de-sistemas-informaticos/practicas/p4-ud-7-confinamiento-de-procesos-con-app-armor-y-se-linux/","title":"Confinamiento de procesos con AppArmor y SELinux","tags":["bastionado-de-redes-y-sistemas","practica","hardening-de-procesos"],"noteIcon":"","dg-note-properties":{"title":"Confinamiento de procesos con AppArmor y SELinux","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","hardening-de-procesos"],"status":"En progreso"}}
---


# 📝 Práctica: Confinamiento de procesos con AppArmor y SELinux

> [!note] Objetivos de la Práctica
> - Conocer el estado y los modos de un sistema MAC.
> - Cambiar perfiles de AppArmor entre *enforce* y *complain*.
> - Consultar contextos, modos y booleanos de SELinux.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/3. Hardening de procesos#3.7 Confinamiento de procesos con sistemas MAC\|Confinamiento de procesos con sistemas MAC]]

## 🛠️ Enunciado

### Parte A. AppArmor (Ubuntu/Debian)

1. Instala las utilidades y perfiles adicionales:

   ```bash
   sudo apt install apparmor-utils apparmor-profiles apparmor-profiles-extra
   ```

2. Consulta el estado, los perfiles cargados y los procesos que los usan:

   ```bash
   sudo aa-status
   ls /etc/apparmor.d/
   ```

3. Elige un perfil, pásalo a modo *complain*, revisa los registros y vuelve a *enforce*:

   ```bash
   sudo aa-complain /ruta/al/programa
   sudo aa-status
   sudo aa-enforce /ruta/al/programa
   ```

4. Modifica una regla del perfil y recárgalo:

   ```bash
   sudo apparmor_parser -r /etc/apparmor.d/<perfil>
   ```

5. Explica la diferencia entre ambos modos y en qué situación usarías cada uno.

### Parte B. SELinux (máquina de pruebas Debian)

> [!warning]
> Hazlo en una máquina virtual con instantánea: activar SELinux puede impedir el arranque si la política no es correcta.

1. Instala y activa SELinux (requiere reiniciar):

   ```bash
   sudo apt install selinux-basics selinux-policy-default
   sudo selinux-activate
   ```

2. Consulta el estado y el modo:

   ```bash
   sestatus -v
   getenforce
   ```

3. Observa los contextos de ficheros e identifica usuario, rol, tipo y nivel:

   ```bash
   ls -Z /etc/passwd
   ```

4. Cambia entre *permissive* y *enforcing*:

   ```bash
   sudo setenforce 0
   getenforce
   sudo setenforce 1
   ```

5. Lista los booleanos y cambia uno de prueba:

   ```bash
   getsebool -a | less
   sudo setsebool <booleano> on
   ```

**Criterio de evaluación asociado:** RA7 b) Se han configurado las características propias del sistema informático para imposibilitar el acceso ilegítimo mediante técnicas de explotación de procesos.

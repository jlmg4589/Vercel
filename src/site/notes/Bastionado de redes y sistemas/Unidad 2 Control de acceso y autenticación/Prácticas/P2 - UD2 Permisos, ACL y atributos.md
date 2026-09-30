---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-2-control-de-acceso-y-autenticacion/practicas/p2-ud-2-permisos-acl-y-atributos/","title":"Permisos, ACL y atributos","tags":["bastionado-de-redes-y-sistemas","practica","control-de-acceso"],"noteIcon":"","dg-note-properties":{"title":"Permisos, ACL y atributos","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","control-de-acceso"],"status":"En progreso"}}
---


# 📝 Práctica: Permisos, ACL y atributos

> [!note] Objetivos de la Práctica
> - Aplicar el control de acceso discrecional (DAC) en Linux con permisos UGO.
> - Usar los permisos especiales (SetUID, SetGID, *sticky bit*) y localizarlos.
> - Definir permisos granulares con ACL (`getfacl`/`setfacl`) y máscara.
> - Proteger ficheros con atributos (`chattr`/`lsattr`).

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Apuntes/5. Control de acceso#5.4.2 Control de acceso discrecional (DAC)\|Control de acceso discrecional]]
- [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Apuntes/5. Control de acceso#5.5 Permisos en Linux\|Permisos en Linux]]
- [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Apuntes/5. Control de acceso#5.6 ACL y atributos en Linux\|ACL y atributos en Linux]]

## 🛠️ Enunciado

Parte de los usuarios `juan` y `maria` (grupo `alumnos`) de la [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Prácticas/P1 - UD2 Gestión segura de usuarios, grupos y contraseñas\|P1]] y crea el grupo `profesores` con el usuario `ana`.

1. **Propietarios y permisos básicos:**

   ```bash
   sudo mkdir -p /srv/proyecto
   sudo chown juan:alumnos /srv/proyecto
   sudo chmod 770 /srv/proyecto
   touch carta.txt
   chmod u=rw,g=r,o= carta.txt      # equivale a 640
   ls -l carta.txt
   ```

   Calcula en octal los permisos `rwxr-x---` y `rw-rw-r--`.

2. **SetGID en directorio compartido:** haz que todo lo creado en `/srv/proyecto` pertenezca al grupo `alumnos`.

   ```bash
   sudo chmod g+s /srv/proyecto
   ```

   Crea un fichero como `maria` y comprueba su grupo.

3. **Sticky bit:** crea `/srv/tmp` con permisos `1777` y comprueba que `maria` no puede borrar un fichero de `juan`.

4. **Búsqueda de permisos peligrosos:**

   ```bash
   find / -type d -perm -002 2>/dev/null   # directorios escribibles por todos
   find / -perm /6000 2>/dev/null          # ficheros SUID/SGID
   ```

   Justifica por qué `passwd` tiene SUID.

5. **ACL:** comprueba el soporte y da a `ana` lectura sobre `carta.txt` y al grupo `profesores` lectura/escritura recursiva sobre `/srv/proyecto`, con herencia:

   ```bash
   grep ACL /boot/config-$(uname -r)
   setfacl -m u:ana:r-- carta.txt
   sudo setfacl -Rm g:profesores:rwx /srv/proyecto
   sudo setfacl -d -m g:profesores:rwx /srv/proyecto
   getfacl carta.txt /srv/proyecto
   ls -l            # observa el signo +
   ```

6. **Máscara:** limita la ACL de `carta.txt` a solo lectura y comprueba el permiso efectivo:

   ```bash
   setfacl -m m::r carta.txt
   getfacl carta.txt
   ```

7. **Eliminar ACL:** `setfacl -x u:ana carta.txt` y después `setfacl -b carta.txt`.

8. **Atributos:**

   ```bash
   sudo chattr +i /srv/proyecto/importante.txt   # inmutable: intenta borrarlo como root
   sudo chattr +a /srv/proyecto/registro.log     # solo añadir: prueba > y >>
   lsattr /srv/proyecto
   sudo chattr -i /srv/proyecto/importante.txt
   ```

## ✅ Entrega

Documento con capturas y respuestas. Explica qué ventaja aporta la ACL frente a crear un grupo nuevo por cada combinación de permisos.

**Criterio de evaluación asociado:** RA2 (control de acceso a los recursos; apoya los criterios a y b).

> [!quote]- Fuentes
> - `C11+-+S4_02+-+usuarios,+grupos,+permisos.pdf`, `C11+-+S4_05+-+ACL+y+Atributos.pdf`, `C11+-+S4_06-Seguridad.pdf`
> - `Bloque 2- Sistemas Control de Acceso.pdf` (apdo. 4.4.2)

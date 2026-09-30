---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-7-configuracion-de-sistemas-informaticos/practicas/p6-ud-7-copias-de-seguridad/","title":"Copias de seguridad","tags":["bastionado-de-redes-y-sistemas","practica","copias-de-seguridad"],"noteIcon":"","dg-note-properties":{"title":"Copias de seguridad","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","copias-de-seguridad"],"status":"En progreso"}}
---


# 📝 Práctica: Copias de seguridad

> [!warning] Práctica basada en fuentes externas
> Las fuentes de BASTIONADO desarrollan la teoría de copias de seguridad pero no incluyen una práctica ni herramientas concretas. Esta práctica se ha elaborado a partir de documentación oficial: [GNU tar – Incremental Dumps](https://www.gnu.org/software/tar/manual/html_node/Incremental-Dumps.html), [rsync(1)](https://man7.org/linux/man-pages/man1/rsync.1.html), [restic documentation](https://restic.readthedocs.io/en/stable/), [crontab(5)](https://man7.org/linux/man-pages/man5/crontab.5.html), [Microsoft Support – Copia de seguridad de tu PC Windows](https://support.microsoft.com/es-es/windows/copia-de-seguridad-de-tu-pc-windows-87a81f8a-78fa-456e-b521-ac0560e32338), [Microsoft Support – Historial de archivos](https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255) y la [Guía de copias de seguridad de INCIBE](https://www.incibe.es/sites/default/files/contenidos/guias/guia-copias-de-seguridad.pdf).

> [!note] Objetivos de la Práctica
> - Realizar copias **completas** e **incrementales** con `tar` y programarlas con `cron`.
> - Sincronizar una copia a otro equipo por SSH con `rsync`.
> - Crear un repositorio **cifrado y deduplicado** con `restic`, aplicar una política de retención y verificarlo.
> - Configurar las copias de seguridad de **Windows** (Historial de archivos).
> - **Restaurar** y comprobar que las copias funcionan (regla 3-2-1-1-0).

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/8. Sistemas de copias de seguridad#8.6 La estrategia 3-2-1\|Estrategia 3-2-1]] y tipos de copia (completa, diferencial, incremental).
- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/8. Sistemas de copias de seguridad\|8. Sistemas de copias de seguridad]] (apartado 8.8, herramientas de copia)
- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/5. Securización de la administración remota\|5. Securización de la administración remota]] (autenticación SSH por clave pública, necesaria para `rsync` desatendido).

## 🛠️ Enunciado

Entorno: dos máquinas virtuales Debian/Ubuntu (`origen` y `backup`, con SSH) y una máquina virtual Windows 11 Pro con un segundo disco virtual.

### Parte 1: Copias completas e incrementales con tar

1. Prepara datos de prueba y un directorio de destino:

   ```bash
   mkdir -p ~/datos && for i in 1 2 3; do echo "fichero $i" > ~/datos/f$i.txt; done
   sudo mkdir -p /backup && sudo chown $USER /backup
   ```

2. Realiza la copia **completa de nivel 0** (crea el fichero de instantánea `.snar`):

   ```bash
   tar -czg /backup/datos.snar -f /backup/datos-full-$(date +%F).tar.gz -C ~ datos
   ```

3. Modifica un fichero, crea otro y haz una copia **incremental**; lista su contenido:

   ```bash
   echo "cambio" >> ~/datos/f1.txt; echo "nuevo" > ~/datos/f4.txt
   tar -czg /backup/datos.snar -f /backup/datos-inc-$(date +%F-%H%M).tar.gz -C ~ datos
   tar -tvzf /backup/datos-inc-*.tar.gz
   ```

4. Programa la incremental cada noche a las 02:00 con `crontab -e` (en crontab el `%` debe escaparse):

   ```text
   0 2 * * * tar -czg /backup/datos.snar -f /backup/datos-inc-$(date +\%F).tar.gz -C /home/usuario datos
   ```

### Parte 2: Copia remota con rsync por SSH

1. Configura acceso por clave pública de `origen` a `backup` (ver P2) y sincroniza:

   ```bash
   rsync -avz --delete /backup/ usuario@backup:/srv/copias/origen/
   ```

2. Explica en el informe el riesgo de `--delete` (un borrado o cifrado en origen se propaga) y por qué esta copia sola **no** protege frente a *ransomware*.

### Parte 3: Repositorio cifrado con restic

1. Instala `restic`, inicializa un repositorio (en local o por SFTP en `backup`) y guarda la contraseña en lugar seguro:

   ```bash
   sudo apt install restic
   restic -r sftp:usuario@backup:/srv/restic init
   restic -r sftp:usuario@backup:/srv/restic backup ~/datos
   ```

2. Haz varios cambios y copias; consulta las instantáneas, aplica una retención y verifica la integridad:

   ```bash
   restic -r sftp:usuario@backup:/srv/restic snapshots
   restic -r sftp:usuario@backup:/srv/restic forget --keep-daily 7 --keep-weekly 4 --prune
   restic -r sftp:usuario@backup:/srv/restic check
   ```

3. Borra `~/datos/f2.txt` y **restáuralo** desde la última instantánea:

   ```bash
   restic -r sftp:usuario@backup:/srv/restic restore latest --target /tmp/restaurado --include /home/usuario/datos/f2.txt
   ```

### Parte 4: Copias de seguridad en Windows 11

1. Formatea el segundo disco como `COPIAS` (Administración de discos).
2. Activa el **Historial de archivos**: **Panel de control → Sistema y seguridad → Historial de archivos → Activar**, eligiendo la unidad `COPIAS`; en **Configuración avanzada** fija la frecuencia (p. ej. cada hora) y la retención.
3. Modifica y borra un documento; recupéralo con **Restaurar archivos personales**.
4. Revisa la app **Copia de seguridad de Windows** (copia de carpetas y configuración en OneDrive) y explica en qué se diferencia del Historial de archivos (local, versionado) en términos de la regla 3-2-1.

### Parte 5: Informe

1. Dibuja el esquema de tu solución e indica cómo cumple **3-2-1-1-0**: 3 copias, 2 soportes, 1 fuera (equipo `backup` o nube), 1 inmutable/desconectada (propón cómo conseguirla) y 0 errores (resultado de `restic check` y de las restauraciones).
2. Adjunta las capturas de las restauraciones realizadas en Linux y Windows.

**Criterio de evaluación asociado:** RA7 e) Se han instalado y configurado sistemas de copias de seguridad.

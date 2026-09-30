---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-6-configuracion-de-dispositivos-para-la-instalacion/practicas/p1-ud-6-particionado-y-cifrado-de-ficheros-en-windows/","title":"Particionado y cifrado de ficheros en Windows","tags":["bastionado-de-redes-y-sistemas","practica","sistemas-de-ficheros"],"noteIcon":"","dg-note-properties":{"title":"Particionado y cifrado de ficheros en Windows","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","sistemas-de-ficheros"],"status":"En progreso"}}
---


# 📝 Práctica: Particionado y cifrado de ficheros en Windows

> [!note] Objetivos de la Práctica
> - Crear una partición dedicada a datos en Windows 10 con Administración de discos.
> - Cifrar archivos y carpetas con EFS y hacer copia de seguridad del certificado.
> - Comprobar el estado del TPM y, si procede, cifrar la unidad con BitLocker.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 6 Configuración de dispositivos para la instalación/Apuntes/4. Seguridad de los sistemas de ficheros#4.4 Particionado\|Particionado]]
- [[Bastionado de redes y sistemas/Unidad 6 Configuración de dispositivos para la instalación/Apuntes/4. Seguridad de los sistemas de ficheros#4.3.2 Cifrado en Windows. EFS\|Cifrado con EFS]]
- [[Bastionado de redes y sistemas/Unidad 6 Configuración de dispositivos para la instalación/Apuntes/5. Hardening de sistemas#5.5 Cifrado de SSD, discos duros o particiones\|BitLocker]]

> [!warning] Realiza la práctica en una **máquina virtual** con Windows 10 Pro/Education (EFS no está disponible en Windows 10 Home).

> [!info] Actualización 2026
> Windows 10 ya no tiene soporte: se puede usar igualmente **Windows 11 Pro/Education** (mismos pasos). Para una VM con Windows 11 hay que habilitar **TPM 2.0 virtual** y **Secure Boot**, requisitos mínimos del sistema (fuente: [Windows 11 specifications – Microsoft](https://www.microsoft.com/en-us/windows/windows-11-specifications)).

## 🛠️ Enunciado

### Parte 1: crear una partición de datos

1. Abre **Crear y formatear particiones del disco duro** desde el menú Inicio.
2. Clic derecho sobre `C:` → **Reducir volumen** e indica el espacio a liberar en MB.
3. Clic derecho sobre el espacio **No asignado** → **Nuevo volumen simple**.
4. Define el tamaño, asigna una letra de unidad, formatea en **NTFS** y pon la etiqueta `DATOS`.
5. Revisa el resumen y finaliza. Haz una captura del resultado.

### Parte 2: cifrar con EFS

1. Crea la carpeta `DATOS:\Confidencial` con un fichero de prueba.
2. Propiedades → **Opciones avanzadas** → **Cifrar contenido para proteger datos** → Aceptar → Aplicar. Elige cifrar la carpeta y su contenido.
3. Comprueba que aparece el **candado** en los iconos.
4. Haz copia del certificado y las claves EFS:
   ```bat
   cipher /X C:\Users\%USERNAME%\Desktop\copia_efs
   ```
5. Crea otro usuario estándar, inicia sesión con él e intenta abrir el fichero. Anota el resultado.

### Parte 3: TPM y BitLocker

1. Consulta **Seguridad de Windows → Seguridad del dispositivo → Procesador de seguridad**.
2. Si hay TPM, cifra la unidad `DATOS` con **BitLocker** y guarda la **clave de recuperación de 48 dígitos**.
3. Explica qué ventaja aporta BitLocker frente a EFS ante la extracción física del disco.

## ✅ Criterios de evaluación asociados

- **RA6 d)** Instalar un sistema utilizando el cifrado del sistema de ficheros para evitar la extracción física de datos.
- **RA6 e)** Particionar el sistema de ficheros para minimizar riesgos de seguridad.

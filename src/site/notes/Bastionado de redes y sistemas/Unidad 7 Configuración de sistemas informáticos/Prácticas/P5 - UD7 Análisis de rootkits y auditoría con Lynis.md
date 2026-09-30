---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-7-configuracion-de-sistemas-informaticos/practicas/p5-ud-7-analisis-de-rootkits-y-auditoria-con-lynis/","title":"Análisis de rootkits y auditoría con Lynis","tags":["bastionado-de-redes-y-sistemas","practica","hardening-de-procesos"],"noteIcon":"","dg-note-properties":{"title":"Análisis de rootkits y auditoría con Lynis","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","hardening-de-procesos"],"status":"En progreso"}}
---


# 📝 Práctica: Análisis de rootkits y auditoría con Lynis

> [!note] Objetivos de la Práctica
> - Buscar rootkits con Chkrootkit y Rootkit Hunter.
> - Auditar el nivel de *hardening* con Lynis.
> - Comparar los resultados y aplicar mejoras.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/6. Prevención y protección frente a virus e intrusiones#6.2.2 Detección de rootkits en GNU/Linux\|Detección de rootkits]]
- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/6. Prevención y protección frente a virus e intrusiones#6.2.3 Auditoría de hardening con Lynis\|Lynis]]
- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/3. Hardening de procesos\|3. Hardening de procesos]]

## 🛠️ Enunciado

Entorno: máquina virtual Ubuntu.

1. Instala y ejecuta Chkrootkit:

   ```bash
   sudo apt install chkrootkit
   sudo chkrootkit
   sudo chkrootkit -q
   ```

2. Instala Rootkit Hunter, actualiza sus firmas y realiza un análisis completo:

   ```bash
   sudo apt install rkhunter
   sudo rkhunter --update
   sudo rkhunter --list tests
   sudo rkhunter --check
   ```

3. Ejecuta una auditoría completa con Lynis y anota el índice de fortificación:

   ```bash
   sudo apt install lynis
   sudo lynis audit system
   ```

4. Compara los resultados de las tres herramientas: ¿coinciden los avisos? ¿Cuáles son falsos positivos conocidos?
5. Aplica al menos tres sugerencias de Lynis (por ejemplo, las relativas a SSH o servicios) y vuelve a ejecutar la auditoría para comprobar si mejora el índice.

**Criterio de evaluación asociado:** RA7 b) Se han configurado las características propias del sistema informático para imposibilitar el acceso ilegítimo mediante técnicas de explotación de procesos.

---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-5-configuracion-de-dispositivos-y-sistemas/practicas/p1-ud-5-cortafuegos-con-iptables/","title":"Cortafuegos con iptables","tags":["bastionado-de-redes-y-sistemas","practica","cortafuegos"],"noteIcon":"","dg-note-properties":{"title":"Cortafuegos con iptables","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","cortafuegos"],"status":"En progreso"}}
---


# 📝 Práctica: Cortafuegos con iptables

> [!note] Objetivos de la Práctica
> - Construir reglas de iptables de forma incremental y comprobar el efecto del orden.
> - Configurar NAT/enmascaramiento para una red interna.
> - Crear un *script* de cortafuegos con política por defecto DROP.
> - Detectar errores de configuración analizando el tráfico.

## 📐 Recordatorio Teórico

- Tablas, cadenas, comandos, parámetros, acciones y módulos: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/10. Configuración segura de cortafuegos, enrutadores y proxies#10.3.2 iptables\|10.3.2]].
- Políticas ACEPTAR/DENEGAR y orden de reglas: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/10. Configuración segura de cortafuegos, enrutadores y proxies#10.3.1 Políticas por defecto\|10.3.1]].
- Depuración con iptraf: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/10. Configuración segura de cortafuegos, enrutadores y proxies#10.5 Depuración de un cortafuegos\|10.5]].
- Limitación de velocidad: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/9. Protección ante ataques DDoS#9.3.3 Conocer el tráfico normal y anormal\|9.3.3]].

**Entorno:** una máquina virtual Linux (Ubuntu/Debian) con servidor web y, para la parte de NAT, una segunda interfaz hacia una red interna 192.168.0.0/16.

## 🛠️ Enunciado

### Parte 1 · Reglas paso a paso

1. Lista las reglas actuales (deberían estar vacías y con política ACCEPT):
   ```bash
   sudo iptables -L
   ```
2. Permite las sesiones ya establecidas:
   ```bash
   sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
   ```
3. Permite SSH y HTTP entrantes:
   ```bash
   sudo iptables -A INPUT -p tcp --dport ssh -j ACCEPT
   sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
   ```
4. Bloquea todo lo demás y lista:
   ```bash
   sudo iptables -A INPUT -j DROP
   sudo iptables -L
   ```
5. Añade la interfaz *loopback*. Comprueba que con `-A` quedaría **después** del DROP y no serviría; insértala en primera posición:
   ```bash
   sudo iptables -I INPUT 1 -i lo -j ACCEPT
   sudo iptables -L -v
   ```
   Explica por qué con `-L` la primera y la última regla parecen iguales y qué aclara `-v`.

### Parte 2 · Limitación y estado

6. Añade una regla que limite las conexiones TCP entrantes a 60 por segundo con ráfaga de 20 y otra que acepte SSH solo en estado NEW/ESTABLISHED por la interfaz `enp0s3`:
   ```bash
   sudo iptables -A INPUT -p tcp -m limit --limit 60/s --limit-burst 20 -j ACCEPT
   sudo iptables -A INPUT -i enp0s3 -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
   ```
   ¿En qué posición deben ir para tener efecto? Reordénalas si es necesario.

### Parte 3 · NAT para la red interna

7. Enmascara el tráfico de la red interna que sale por `eth0` y permite el reenvío:
   ```bash
   sudo iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o eth0 -j MASQUERADE
   sudo iptables -A FORWARD -s 192.168.0.0/16 -o eth0 -j ACCEPT
   sudo iptables -A FORWARD -d 192.168.0.0/16 -m state --state ESTABLISHED,RELATED -i eth0 -j ACCEPT
   sudo iptables --list-rules
   ```

### Parte 4 · Script con política DROP

8. Crea `iptables-script.sh` que cumpla:
   - *Flush* de reglas (`-F`, `-X`, `-Z`, `-t nat -F`) y políticas **DROP** en INPUT, OUTPUT y FORWARD.
   - *Loopback* sin restricciones.
   - Acceso total a la IP del jefe 195.65.34.234.
   - MySQL (3306) solo desde 231.45.134.23 y FTP (20:21) solo desde 80.37.45.194.
   - Servidor web en el puerto 80.
   - Navegación HTTP y HTTPS desde el propio equipo.
   - DNS 211.95.64.39 (UDP 53) y NTP 130.206.3.166 (UDP 123).
   - Recuerda: todo lo que entra por INPUT necesita su respuesta en OUTPUT (`-m state --state RELATED,ESTABLISHED`).
9. Ejecútalo y verifica:
   ```bash
   chmod +x iptables-script.sh
   sudo ./iptables-script.sh
   sudo iptables -L -n
   ```

### Parte 5 · Detección de errores mediante el tráfico

10. Elimina deliberadamente la regla de salida de respuestas del servidor web (OUTPUT `--sport 80`). Desde otro equipo intenta acceder a la web y observa con **iptraf** o **Wireshark** el tráfico: ¿qué *flags* aparecen y en qué sentido? Relaciónalo con el *three-way handshake*.
11. Revisa los contadores con `sudo iptables -L -v -n` y localiza la regla que está descartando los paquetes. Corrige el error.

> [!tip] Ayuda
> Para la Parte 4 puedes partir del *script* de [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/10. Configuración segura de cortafuegos, enrutadores y proxies#10.3.2.5 Scripts de configuración\|10.3.2.5]] y añadir las reglas de MySQL, FTP y la IP del jefe (cada una con su pareja INPUT/OUTPUT).

## ✅ Criterios de evaluación asociados

- **RA5 a)** Se han configurado dispositivos de seguridad perimetral acorde a una serie de requisitos de seguridad.
- **RA5 b)** Se han detectado errores de configuración de dispositivos de red mediante el análisis de tráfico.
- **RA5 d)** Se han implementado contramedidas frente a comportamientos no deseados en una red.

> [!quote]- Fuentes
> - `00-Unidad 3 - Bastionado de redes de area local.pdf` (Iptables: un ejemplo, enmascaramiento IP, scripts de configuración, iptraf)
> - `C11+-+S2_01+-+iptables-1.pdf` (opciones y módulos limit y state)

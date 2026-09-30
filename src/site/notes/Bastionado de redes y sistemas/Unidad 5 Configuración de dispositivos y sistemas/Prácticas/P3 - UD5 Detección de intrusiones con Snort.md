---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-5-configuracion-de-dispositivos-y-sistemas/practicas/p3-ud-5-deteccion-de-intrusiones-con-snort/","title":"Detección de intrusiones con Snort","tags":["bastionado-de-redes-y-sistemas","practica","ids-ips"],"noteIcon":"","dg-note-properties":{"title":"Detección de intrusiones con Snort","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","ids-ips"],"status":"En progreso"}}
---


# 📝 Práctica: Detección de intrusiones con Snort

> [!note] Objetivos de la Práctica
> - Instalar y configurar Snort como NIDS y, opcionalmente, como IPS *inline*.
> - Escribir reglas propias y comprobar que generan alertas.
> - Enviar las alertas a un gestor de eventos (Barnyard2 + Snorby).

## 📐 Recordatorio Teórico

- Tipos de IDS/IPS, ubicación y funcionamiento: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/13. Herramientas de monitorización IDS e IPS\|13. Herramientas de monitorización IDS e IPS]].
- Componentes y sintaxis de reglas de Snort: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/13. Herramientas de monitorización IDS e IPS#13.7 Snort\|13.7]].
- Gestión de alertas y SIEM: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/14. SIEM\|14. SIEM]].

**Entorno (basado en el laboratorio de INCIBE):** una VM Ubuntu Server con Snort y dos interfaces (`eth0`, `eth1`) entre dos redes; una VM atacante; opcionalmente otra VM con Snorby.

## 🛠️ Enunciado

### Parte 1 · Instalación como IDS

1. Instala Snort (configuración en `/etc/snort`):
   ```bash
   sudo apt install snort
   ```
2. En `/etc/snort/snort.conf` define la red protegida:
   ```text
   ipvar HOME_NET 192.168.1.0/24
   ipvar EXTERNAL_NET !$HOME_NET
   ```

### Parte 2 · Reglas propias

3. Añade a tu fichero de reglas locales (p. ej. `local.rules`, incluido desde `snort.conf`) las reglas siguientes, recordando que los `sid` propios deben ser mayores de 1.000.000:
   ```text
   alert ip 8.8.8.8 53 -> 192.168.0.0/24 53 (msg:"DNS"; sid:1000001;)
   alert tcp any any -> 192.168.1.10 21 (content:"USER root"; msg:"Acceso root por FTP"; sid:1000002;)
   alert tcp any any -> 192.168.1.0/24 80 (content:"cgi-bin/phf"; offset:3; depth:22; msg:"CGI-PHF access"; sid:1000003;)
   ```
4. Escribe además reglas propias para:
   - Alertar de cualquier `ping` (ICMP) hacia `$HOME_NET`.
   - Alertar de intentos de conexión a Telnet (23) desde `$EXTERNAL_NET`.
   - Alertar si en una petición HTTP aparece la cadena `union select` sin distinguir mayúsculas (opción `nocase`).
5. Ejecuta Snort en modo IDS mostrando alertas por consola (orientativo, conocimiento general, verificar):
   ```bash
   sudo snort -A console -q -c /etc/snort/snort.conf -i eth0
   ```
6. Desde la máquina atacante genera el tráfico de cada regla (`ping`, `telnet`, `ftp` con usuario root, `curl` con la URL maliciosa) y documenta las alertas obtenidas.

### Parte 3 · Modo IPS inline (opcional)

7. Instala las dependencias y la librería DAQ (indicando la versión):
   ```bash
   sudo apt install libdnet libdnet-dev libpcap-dev make automake gc flex bison libdumbnet-dev
   sudo ln -s /usr/include/dumbnet.h /usr/include/dnet.h && sudo ldconfig
   wget https://www.snort.org/downloads/snort/daq-X.X.X.tar.gz
   tar zxf daq-X.X.X.tar.gz && cd daq-X.X.X
   ./configure && make && sudo make install && sudo ldconfig
   ```
8. Crea un *bridge* transparente (interfaces sin IP) en `/etc/network/interfaces`:
   ```text
   auto br0
   iface br0 inet manual
          bridge-ports eth0 eth1
          bridge_stp off
          bridge_fd 0
   ```
9. En `snort.conf` activa el modo *inline* y en `snort.debian.conf` indica las interfaces:
   ```text
   config daq: afpacket
   config daq_mode: inline
   DEBIAN_SNORT_INTERFACES="eth0:eth1"
   ```
10. Cambia la acción de la regla de Telnet a `drop`, desactiva el *bridge* y lanza Snort como IPS:
    ```bash
    sudo ifconfig br0 down
    sudo snort -Q -i eth0:eth1 -c /etc/snort/snort.conf
    # al terminar:
    sudo ifconfig br0 up -arp
    ```
11. Comprueba desde el atacante que Telnet queda bloqueado y el resto del tráfico pasa.

> [!warning] Precaución
> Según INCIBE, antes de bloquear tráfico hay que analizarlo en modo IDS o *inline test* para no cortar tráfico legítimo.

### Parte 4 · Envío de alertas a Snorby (ampliación)

12. Configura la salida unificada en `snort.conf` y Barnyard2 hacia la base de datos MySQL de Snorby:
    ```text
    output unified2: filename snort.u2, limit 128
    output database: alert, mysql, user=usuario password=contraseña dbname=snorby host=host_remoto
    ```
    ```bash
    sudo touch /var/log/snort/barnyard2.waldo
    sudo barnyard2 -c /etc/snort/barnyard2.conf -d /var/log/snort -f snort.u2 -w /var/log/snort/barnyard2.waldo
    ```
13. Instala Snorby siguiendo la guía de INCIBE (BD `snorby`, Apache + Passenger, `RAILS_ENV=production bundle exec rake snorby:setup`), accede con `snorby@example.com` / `snorby`, **cambia la contraseña** y localiza las alertas de la Parte 2.

## ✅ Criterios de evaluación asociados

- **RA5 d)** Se han implementado contramedidas frente a comportamientos no deseados en una red.
- **RA5 e)** Se han caracterizado, instalado y configurado diferentes herramientas de monitorización.

> [!quote]- Fuentes
> - `C11+-+S2_03+-+snort.pdf` (componentes y reglas de Snort)
> - `incibe-diseno_configuracion_ips_ids_siem_en_sci.pdf` (apdo. 5: bridge, instalación de Snort y DAQ, snort.conf, ejecución inline, Barnyard2, Snorby)

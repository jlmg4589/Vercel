---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-5-configuracion-de-dispositivos-y-sistemas/practicas/p2-ud-5-cortafuegos-con-nftables/","title":"Cortafuegos con nftables","tags":["bastionado-de-redes-y-sistemas","practica","cortafuegos"],"noteIcon":"","dg-note-properties":{"title":"Cortafuegos con nftables","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","cortafuegos"],"status":"En progreso"}}
---


# 📝 Práctica: Cortafuegos con nftables

> [!note] Objetivos de la Práctica
> - Instalar nftables y gestionar tablas, cadenas y reglas con `nft`.
> - Reproducir con nftables la política del cortafuegos de la práctica de iptables.
> - Registrar (`log`) y contar (`counter`) tráfico para analizar comportamientos no deseados.

## 📐 Recordatorio Teórico

- Familias, tablas, cadenas base/normales, selectores y acciones: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/10. Configuración segura de cortafuegos, enrutadores y proxies#10.4 nftables\|10.4]].
- Registro de eventos del cortafuegos: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Apuntes/8. Herramientas de almacenamiento de logs#8.5.1 Registro en el cortafuegos\|8.5.1]].
- Práctica previa: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Prácticas/P1 - UD5 Cortafuegos con iptables\|P1 - UD5 Cortafuegos con iptables]].

## 🛠️ Enunciado

1. **Instalación** (evitando dos sistemas de filtrado a la vez):
   ```bash
   sudo apt-get update && sudo apt-get upgrade
   sudo apt-get remove iptables
   sudo apt-get install nftables
   ```
2. **Tabla**: crea una tabla de la familia `inet` (IPv4 e IPv6) y lista las tablas:
   ```bash
   sudo nft add table inet filtro
   sudo nft list tables
   ```
3. **Cadenas base** con *hook* y prioridad para entrada, reenvío y salida (tipo `filter`):
   ```bash
   sudo nft add chain inet filtro entrada { type filter hook input priority 0 \; }
   sudo nft add chain inet filtro reenvio { type filter hook forward priority 0 \; }
   sudo nft add chain inet filtro salida  { type filter hook output priority 0 \; }
   ```
4. **Reglas básicas** de entrada:
   ```bash
   sudo nft add rule inet filtro entrada iifname "lo" accept
   sudo nft add rule inet filtro entrada ct state established,related accept
   sudo nft add rule inet filtro entrada tcp dport 22 accept comment \"SSH\"
   sudo nft add rule inet filtro entrada tcp dport 80 accept comment \"Web\"
   sudo nft add rule inet filtro entrada icmp type echo-request limit rate 5/second accept
   ```
5. **Registro y descarte** del resto, con contador:
   ```bash
   sudo nft add rule inet filtro entrada counter log prefix \"NFT-DROP \" drop
   sudo nft list ruleset
   ```
6. **Uso de *handle***: lista con `sudo nft -a list chain inet filtro entrada`, localiza el *handle* de la regla ICMP e inserta antes una regla que acepte HTTPS (`insert rule … position <handle>` o `handle`). Elimina después la regla ICMP con `delete rule … handle <n>`.
7. **Cadena normal**: crea una cadena `servicios` sin *hook*, mueve a ella las reglas de SSH/HTTP y salta a ella desde `entrada` con `jump servicios`.
8. **Traducción**: usa `iptables-translate` para convertir tres reglas del *script* de [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Prácticas/P1 - UD5 Cortafuegos con iptables\|P1 - UD5 Cortafuegos con iptables]] y compáralas con las tuyas:
   ```bash
   iptables-translate -A INPUT -p tcp --dport 80 -j ACCEPT
   ```
9. **Persistencia**: guarda la configuración en `/etc/nftables.conf` para que se cargue al iniciar:
   ```bash
   sudo nft list ruleset | sudo tee /etc/nftables.conf
   ```
10. **Análisis de logs**: desde otro equipo lanza conexiones a puertos cerrados (p. ej. 23, 3306). Consulta los mensajes con prefijo `NFT-DROP` en el registro del sistema y los contadores de la regla. Indica qué IP y puertos aparecen y qué comportamiento sugieren.

> [!warning] Falta fuente
> Las fuentes no incluyen el formato completo de `/etc/nftables.conf` ni la consulta de los mensajes de `log` (orientativamente `journalctl -k` o `/var/log/kern.log`, conocimiento general, verificar).

## ✅ Criterios de evaluación asociados

- **RA5 a)** Se han configurado dispositivos de seguridad perimetral acorde a una serie de requisitos de seguridad.
- **RA5 c)** Se han identificado comportamientos no deseados en una red a través del análisis de los registros (*logs*) de un cortafuegos.

> [!quote]- Fuentes
> - `C11+-+S2_02+-+nftables.pdf` (tablas, familias, cadenas, reglas, selectores, acciones, iptables-translate)
> - `UT 5-Configuración de Dispositivos de SI.pdf` (apdo. 9.1: instalación de NFTables y familias)

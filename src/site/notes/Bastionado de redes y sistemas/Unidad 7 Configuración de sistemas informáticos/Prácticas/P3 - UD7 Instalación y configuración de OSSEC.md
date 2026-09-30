---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-7-configuracion-de-sistemas-informaticos/practicas/p3-ud-7-instalacion-y-configuracion-de-ossec/","title":"Instalación y configuración de OSSEC","tags":["bastionado-de-redes-y-sistemas","practica","hids"],"noteIcon":"","dg-note-properties":{"title":"Instalación y configuración de OSSEC","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","hids"],"status":"En progreso"}}
---


# 📝 Práctica: Instalación y configuración de OSSEC

> [!note] Objetivos de la Práctica
> - Instalar un HIDS OSSEC en modo servidor y agente.
> - Establecer la comunicación cifrada mediante PSK.
> - Activar la monitorización de integridad en tiempo real y comprobar las alertas.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/6. Prevención y protección frente a virus e intrusiones#6.4 OSSEC\|OSSEC]]

## 🛠️ Enunciado

Entorno: dos máquinas Ubuntu 20.04 (o Debian) en la misma red: **servidor** y **cliente**.

### Paso 1. Repositorio (ambas máquinas)

```bash
wget -q -O - https://updates.atomicorp.com/installers/atomic | sudo bash
sudo apt update
sudo apt install inotify-tools
```

### Paso 2. Servidor

```bash
sudo apt install ossec-hids-server
sudo service ossec start
sudo service ossec status
sudo /var/ossec/bin/manage_agents
#   A -> añadir el agente (nombre e IP del cliente)
#   E -> extraer la clave del agente y copiarla
```

### Paso 3. Cliente (agente)

```bash
sudo apt install ossec-hids-agent
sudo touch /var/ossec/queue/rids/sender
sudo /var/ossec/bin/manage_agents
#   I -> importar la clave copiada del servidor
```

Edita `/var/ossec/etc/ossec.conf` e indica la IP del servidor; después:

```bash
sudo service ossec start
```

### Paso 4. Integridad en tiempo real (cliente)

En `/var/ossec/etc/ossec.conf` cambia:

```xml
<directories check_all="yes">/etc,/usr/bin,/usr/sbin</directories>
```

por:

```xml
<directories realtime="yes" check_all="yes">/etc,/usr/bin,/usr/sbin</directories>
```

```bash
sudo service ossec restart
```

### Paso 5. Pruebas

1. En el servidor, verifica que el agente está activo con `/var/ossec/bin/agent_control`.
2. En el servidor, deja abierto el log de alertas:

   ```bash
   sudo tail -f /var/ossec/logs/alerts/alerts.log
   ```

3. En el cliente, provoca varios intentos fallidos de `sudo` y observa las alertas.
4. En el cliente, modifica `/etc/passwd` (p. ej. añadiendo un usuario de prueba) y observa la alerta de integridad centralizada en el servidor.
5. Explica en el informe por qué no se recomienda activar la **respuesta activa** sin conocer bien las alertas del entorno.

**Criterio de evaluación asociado:** RA7 d) Se ha instalado y configurado un Sistema de detección de intrusos en un Host (HIDS) en el sistema informático.

---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-4-diseno-de-redes-seguras/practicas/p1-ud-4-instalacion-y-configuracion-de-gns-3/","title":"Instalación y configuración de GNS3","tags":["bastionado-de-redes-y-sistemas","practica","gns3"],"noteIcon":"","dg-note-properties":{"title":"Instalación y configuración de GNS3","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","gns3"],"status":"En progreso"}}
---


# 📝 Práctica: Instalación y configuración de GNS3

> [!note] Objetivos de la Práctica
> - Instalar el simulador de redes **GNS3** y sus componentes.
> - Configurar el servidor local de GNS3.
> - Añadir una imagen IOS de un router Cisco (7200) para usarla en las topologías de la unidad.

## 📐 Recordatorio Teórico

GNS3 permite simular topologías de red con imágenes de fabricantes como Cisco o Juniper. Lo usaremos para practicar la [[Bastionado de redes y sistemas/Unidad 4 Diseño de redes seguras/Apuntes/5. Segmentación de redes\|segmentación]], el enrutamiento y las [[Bastionado de redes y sistemas/Unidad 4 Diseño de redes seguras/Apuntes/6. Redes virtuales (VLAN)\|VLAN]].

## 🛠️ Enunciado

### 1. Comprobar los requisitos

| Elemento | Mínimo | Recomendado | Óptimo |
| --- | :-: | :-: | :-: |
| Sistema | Windows 7 SP1 64 bits o superior (también Mac y Linux) | ídem | ídem |
| Procesador | 2 núcleos lógicos | 4 núcleos (AMD-V/RVI o Intel VT-x/EPT) | 8 núcleos o más (i7/i9, R7/R9) |
| Virtualización | Extensiones activas en la BIOS | ídem | ídem |
| RAM | 4 GB | 16 GB | 32 GB |
| Disco | 1 GB libre | SSD, 35 GB | SSD, 80 GB |

### 2. Descargar GNS3

1. Ir a `https://www.gns3.com/` → **Free Download** (con registro: *Sign Up* / *Login*; propósito *Education and Training*).
2. Elegir el sistema operativo (Windows, Mac o Linux).

### 3. Instalar

1. Ejecutar el instalador con permisos de administrador y aceptar la licencia (**Agree**).
2. Dejar por defecto la carpeta del menú Inicio.
3. Seleccionar los componentes:

| Componente | Función |
| --- | --- |
| GNS3 | Simulador gráfico de red |
| WinPCAP / Npcap | Envío y captura de paquetes |
| Wireshark | Analizador de paquetes |
| Dynamips | Emulador de routers Cisco |
| QEMU | Ejecuta máquinas virtuales |
| VPCS | Simulador de terminales (PC) |
| Cpulimit | Limita el uso de CPU de un proceso |
| TightVNC Viewer | Control remoto de máquinas virtuales |
| SolarWinds Response | Analizador de paquetes con Wireshark |

4. Dejar la carpeta de instalación por defecto, permitir la instalación de Visual C++ y de las descargas necesarias (p. ej. Wireshark). SolarWinds Standard Toolset es opcional.
5. Finalizar con **Start GNS3**.

### 4. Asistente inicial (Setup Wizard)

Opciones:

- **Run modern IOS (IOSv o IOU), ASA y appliances de otros fabricantes:** requiere la máquina virtual **GNS3 VM** (VMware o VirtualBox, local o remota). Recomendada para topologías avanzadas.
- **Run only legacy IOS on my computer:** servidor local en el propio PC (limitado, pero suficiente para empezar).
- **Run everything on a remote server:** usuarios avanzados.

### 5. Configurar el servidor local

Elegir *Run only legacy IOS on my computer* con:

- **Server path:** ruta por defecto de `gns3server.exe`.
- **Host binding:** `127.0.0.1` (loopback).
- **Port:** `3080` TCP.

### 6. Añadir la imagen IOS del router Cisco 7200 (Dynamips)

1. *New appliance template* → **Add an IOS router using a real IOS image (supported by Dynamips)**.
2. **Browse** y seleccionar la imagen (en el tutorial: `c7200-adventerprisek9-mz.124-24.T5.bin`). Las imágenes IOS deben obtenerse legalmente.
3. ¿Descomprimir? **Sí** → arranque más rápido (la imagen ocupa un 225 % más); **No** → se descomprime en cada arranque.
4. Nombre y plataforma (**7200**).
5. RAM: la indicada por GNS3 (esta imagen requiere al menos **512 MB**; se puede comprobar en *Cisco Feature Navigator*).
6. Interfaces por slot (slot 0: C7200-IO-2FE…; slots 1-6: PA-FE-TX, PA-2FE-TX, PA-GE, PA-4T+, PA-8T…).
7. Buscar un valor **Idle-PC** (imprescindible para no saturar la CPU) y **Finish**.
8. Crear un proyecto (*File → New Blank Project*) y arrastrar el router al área de trabajo.

### ✅ Entregable

Captura de GNS3 con un proyecto nuevo y un router 7200 arrancado mostrando el prompt `Router>`.

**Criterio de evaluación asociado:** RA4 a) (preparación del entorno de simulación para segmentar redes con dispositivos de enrutamiento).

> [!quote]- Fuentes
> - `GNS3-Instalación y Configuración.pdf`

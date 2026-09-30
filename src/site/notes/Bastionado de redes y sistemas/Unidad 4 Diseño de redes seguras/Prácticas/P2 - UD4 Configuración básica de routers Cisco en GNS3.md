---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-4-diseno-de-redes-seguras/practicas/p2-ud-4-configuracion-basica-de-routers-cisco-en-gns-3/","title":"Configuración básica de routers Cisco en GNS3","tags":["bastionado-de-redes-y-sistemas","practica","gns3"],"noteIcon":"","dg-note-properties":{"title":"Configuración básica de routers Cisco en GNS3","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","gns3"],"status":"En progreso"}}
---


# 📝 Práctica: Configuración básica de routers Cisco en GNS3

> [!note] Objetivos de la Práctica
> - Configurar direcciones IP en las interfaces de routers Cisco.
> - Configurar **rutas estáticas** para comunicar subredes distintas.
> - Verificar la configuración y la conectividad, y guardar la topología.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 4 Diseño de redes seguras/Apuntes/5. Segmentación de redes#5.4.2 Tablas de enrutamiento\|Tablas de enrutamiento y rutas estáticas]]
- [[Bastionado de redes y sistemas/Unidad 4 Diseño de redes seguras/Apuntes/4. Protocolo TCP-IP#4.9.3 Máscara de subred\|Máscara de subred]]: se usan subredes /30 (255.255.255.252), típicas de enlaces entre routers (2 hosts útiles).

## 🛠️ Enunciado

### Topología

```text
R5 f0/0 ──────── f0/0 R6 f0/1 ──────── f0/0 R7
10.0.0.1/30   10.0.0.2/30   10.0.0.9/30   10.0.0.10/30
     (red 10.0.0.0/30)            (red 10.0.0.8/30)
```

### 1. Configurar las interfaces

**Router 5**

```text
Router#configure terminal
Router(config)#hostname R5
R5(config)#int f0/0
R5(config-if)#ip address 10.0.0.1 255.255.255.252
R5(config-if)#no shutdown
R5(config-if)#exit
R5(config)#exit
R5#wr
```

**Router 6** (dos interfaces)

```text
Router#configure terminal
Router(config)#hostname R6
R6(config)#int f0/0
R6(config-if)#ip address 10.0.0.2 255.255.255.252
R6(config-if)#no shutdown
R6(config-if)#exit
R6(config)#int f0/1
R6(config-if)#ip address 10.0.0.9 255.255.255.252
R6(config-if)#no shutdown
R6(config-if)#exit
R6(config)#exit
R6#wr
```

**Router 7**

```text
Router#configure terminal
Router(config)#hostname R7
R7(config)#int f0/0
R7(config-if)#ip address 10.0.0.10 255.255.255.252
R7(config-if)#no shutdown
R7(config-if)#exit
R7(config)#exit
R7#wr
```

### 2. Configurar las rutas estáticas

R5 y R7 necesitan saber cómo llegar a la subred que no tienen conectada directamente:

```text
R5#configure terminal
R5(config)#ip route 10.0.0.8 255.255.255.252 10.0.0.2
R5(config)#exit
R5#wr

R7#configure terminal
R7(config)#ip route 10.0.0.0 255.255.255.252 10.0.0.9
R7(config)#exit
R7#wr
```

> [!note] Corrección respecto a la fuente
> El comando `ip route` se introduce en modo de configuración **global** (`(config)#`), no dentro de una interfaz.

### 3. Verificar la configuración

```text
R6#show running-config
interface FastEthernet0/0
 ip address 10.0.0.2 255.255.255.252
 duplex auto
 speed auto
!
interface FastEthernet0/1
 ip address 10.0.0.9 255.255.255.252
 duplex auto
 speed auto
```

### 4. Verificar la conectividad

```text
R5#ping 10.0.0.9
R5#ping 10.0.0.10
R7#ping 10.0.0.1
R7#ping 10.0.0.2
```

Resultado esperado: `!!!!!` y `Success rate is 100 percent (5/5)`.

### 5. Guardar la topología

1. En cada router o switch: `copy running-config startup-config` (equivale a `wr`).
2. En los PC (VPCS) modificados: `save`.
3. Guardar el proyecto de GNS3.

### ✅ Entregable

Capturas de `show running-config`, `show ip route` (conocimiento general, verificar) y de los `ping` correctos.

**Criterio de evaluación asociado:** RA4 a) (segmentación física con dispositivos de enrutamiento).

> [!quote]- Fuentes
> - `GNS3-Configurar Router Cisco y Guardar Topologia.pdf`

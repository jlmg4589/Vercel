---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-4-diseno-de-redes-seguras/practicas/p3-ud-4-simulacion-de-un-switch-cisco-con-vlan-trunk-y-vtp/","title":"Simulación de un switch Cisco con VLAN, trunk y VTP","tags":["bastionado-de-redes-y-sistemas","practica","vlan"],"noteIcon":"","dg-note-properties":{"title":"Simulación de un switch Cisco con VLAN, trunk y VTP","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","vlan"],"status":"En progreso"}}
---


# 📝 Práctica: Simulación de un switch Cisco con VLAN, trunk y VTP

> [!note] Objetivos de la Práctica
> - Simular un switch Cisco en GNS3 usando un router 3725 con el módulo **NM-16ESW**.
> - Crear VLAN y asignarles puertos de acceso.
> - Configurar un enlace **troncal** entre dos switches y propagar las VLAN con **VTP**.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 4 Diseño de redes seguras/Apuntes/6. Redes virtuales (VLAN)\|6. Redes virtuales (VLAN)]]: VLAN, puertos de acceso y troncales, 802.1Q.
- [[Bastionado de redes y sistemas/Unidad 4 Diseño de redes seguras/Apuntes/6. Redes virtuales (VLAN)#6.5 Configuración de VLAN en Cisco\|Configuración de VLAN en Cisco y VTP]].

El switch que trae GNS3 es muy limitado. Un router Cisco 3700 con el módulo NM-16ESW aporta un switch de 16 puertos que admite VLAN, trunk, VTP, agregación de puertos (EtherChannel), *port mirroring*, etc.

## 🛠️ Enunciado

### 1. Preparar el entorno

1. Instalar la imagen IOS del router **3725** (ajustando el **Idle-PC**), añadir un router y activar en su configuración el módulo **NM-16ESW**.
2. Opcional: cambiar su símbolo por el de un switch (*Edit → Symbol manager*).
3. Activar los 16 puertos (FastEthernet 1/0 - 15):

```text
R1#show ip interface brief
R1#configure terminal
R1(config)#hostname sw1
sw1(config)#interface range FastEthernet 1/0 - 15
sw1(config-if-range)#no shutdown
sw1(config-if-range)#switchport
```

4. Conectar dos VPCS al switch en la misma red y comprobar la conectividad:

```text
VPCS[1]> ping 192.168.1.2
```

### 2. Crear las VLAN en sw1 y asignar puertos

| VLAN | Nombre | Puerto | Red |
| :-: | :-: | :-: | :-: |
| 10 | prof | FA1/0 | 192.168.10.0/24 |
| 20 | alum | FA1/1 | 192.168.20.0/24 |

```text
sw1#vlan database
sw1(vlan)#vlan 10 name prof
sw1(vlan)#vlan 20 name alum
sw1(vlan)#exit
sw1#show vlan-switch

sw1(config)#interface FA1/0
sw1(config-if)#switchport access vlan 10
sw1(config)#interface FA1/1
sw1(config-if)#switchport access vlan 20
```

### 3. Configurar sw1 como servidor VTP

```text
sw1(config)#vtp mode server
sw1(config)#vtp domain local
```

### 4. Configurar el troncal (FA1/15)

```text
sw1(config)#interface FastEthernet 1/15
sw1(config-if)#switchport mode trunk
sw1(config-if)#switchport trunk allowed vlan 1-1005
```

### 5. Configurar sw2 como cliente VTP

```text
sw2(config)#vtp mode client
sw2(config)#vtp domain local

sw2(config)#interface FastEthernet 1/15
sw2(config-if)#switchport mode trunk
sw2(config-if)#switchport trunk allowed vlan 1-1005

sw2(config)#interface FA1/0
sw2(config-if)#switchport access vlan 10
sw2(config)#interface FA1/1
sw2(config-if)#switchport access vlan 20

sw2#show vlan-switch
```

### 6. Verificar

```text
sw1#show vlan-switch
sw1#show interface trunk
VPCS[1]> ping 192.168.10.2
VPCS[3]> ping 192.168.20.2
```

Los equipos de la misma VLAN se comunican entre sí aunque estén en switches distintos.

> [!tip] Para pensar
> ¿Puede VPCS[1] (VLAN 10) hacer ping a un equipo de la VLAN 20? ¿Qué haría falta? (ver [[Bastionado de redes y sistemas/Unidad 4 Diseño de redes seguras/Prácticas/P5 - UD4 Enrutamiento entre VLAN\|P5 - UD4 Enrutamiento entre VLAN]]).

### ✅ Entregable

Capturas de `show vlan-switch` en ambos switches, `show interface trunk` y los `ping`.

**Criterio de evaluación asociado:** RA4 b) (segmentación lógica mediante VLAN).

> [!quote]- Fuentes
> - `GNS3-Simulando Swicht Cisco con Router.pdf`

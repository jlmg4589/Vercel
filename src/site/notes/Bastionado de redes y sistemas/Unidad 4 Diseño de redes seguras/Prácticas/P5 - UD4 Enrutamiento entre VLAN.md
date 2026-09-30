---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-4-diseno-de-redes-seguras/practicas/p5-ud-4-enrutamiento-entre-vlan/","title":"Enrutamiento entre VLAN","tags":["bastionado-de-redes-y-sistemas","practica","vlan"],"noteIcon":"","dg-note-properties":{"title":"Enrutamiento entre VLAN","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","vlan"],"status":"En progreso"}}
---


# 📝 Práctica: Enrutamiento entre VLAN

> [!note] Objetivos de la Práctica
> - Enrutar entre dos VLAN usando un **router** (*router-on-a-stick*).
> - Enrutar entre dos VLAN usando un **switch de capa 3** con SVI.
> - Comparar ambas soluciones.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 4 Diseño de redes seguras/Apuntes/6. Redes virtuales (VLAN)#6.6 Enrutamiento entre VLAN\|Enrutamiento entre VLAN]]
- Cada VLAN es una subred distinta; hace falta un dispositivo de capa 3 para comunicarlas. Los encapsulados troncales son dot1Q e ISL (Packet Tracer solo permite dot1Q).

## 🛠️ Enunciado

Puede realizarse en Cisco Packet Tracer o en GNS3.

| Equipo | VLAN | IP | Puerto del switch |
| :-: | :-: | :-: | :-: |
| H1 | 10 | 192.168.10.1/24 (gw 192.168.10.254) | Fa0/1 |
| H2 | 20 | 192.168.20.1/24 (gw 192.168.20.254) | Fa0/2 |

### Parte 1: Router-on-a-stick

```text
H1 ── Fa0/1 ┐
            SW1 Fa0/3 ══ trunk ══ Fa0/0 (Fa0/0.10, Fa0/0.20) R1
H2 ── Fa0/2 ┘
```

1. En SW1 (con las VLAN 10 y 20 ya creadas y Fa0/1 y Fa0/2 asignadas), configurar el troncal:

```text
SW1(config)#interface fa0/3
SW1(config-if)#switchport mode trunk
SW1(config-if)#switchport trunk allowed vlan 10,20
```

2. En R1, crear una **subinterfaz** por VLAN sobre Fa0/0:

```text
R1(config)#interface FastEthernet 0/0.10
R1(config-subif)#encapsulation dot1Q 10
R1(config-subif)#ip address 192.168.10.254 255.255.255.0
R1(config-subif)#exit
R1(config)#interface FastEthernet 0/0.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip address 192.168.20.254 255.255.255.0
R1(config-subif)#no shutdown
R1(config-subif)#exit
R1(config)#interface FastEthernet 0/0
R1(config-if)#no shutdown
```

3. Comprobar con `ping` de H1 a H2.

### Parte 2: Switch de capa 3

```text
H1 ── Fa0/1 ┐
            SW1 (capa 3)
H2 ── Fa0/2 ┘
```

Un switch de capa 3 permite crear una **SVI** por VLAN con su IP (gateway de los equipos) y enrutar entre ellas:

```text
SW1(config)#ip routing
SW1(config)#interface vlan 10
SW1(config-if)#ip address 192.168.10.254 255.255.255.0
SW1(config-if)#no shutdown
SW1(config-if)#exit
SW1(config)#interface vlan 20
SW1(config-if)#ip address 192.168.20.254 255.255.255.0
SW1(config-if)#no shutdown
```

`ip routing` activa el enrutado. En un switch de capa 2 la SVI solo sirve para la gestión remota.

4. Comprobar con `ping` de H1 a H2.

### Cuestiones

1. ¿Qué ventaja tiene cada solución en cuanto a número de dispositivos y enlaces?
2. ¿En qué punto de cada topología aplicarías una ACL para impedir que la VLAN 20 acceda a la VLAN 10?

### ✅ Entregable

Configuraciones de ambos escenarios y capturas de los `ping` entre VLAN.

**Criterios de evaluación asociados:** RA4 b) (VLAN) y a) (dispositivos de enrutamiento).

> [!quote]- Fuentes
> - `Enrutar entre VLANS.pdf` (págs. 1-2)

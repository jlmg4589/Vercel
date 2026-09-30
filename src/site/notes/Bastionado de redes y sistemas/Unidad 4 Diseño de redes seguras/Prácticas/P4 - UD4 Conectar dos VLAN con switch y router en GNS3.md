---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-4-diseno-de-redes-seguras/practicas/p4-ud-4-conectar-dos-vlan-con-switch-y-router-en-gns-3/","title":"Conectar dos VLAN con switch y router en GNS3","tags":["bastionado-de-redes-y-sistemas","practica","vlan"],"noteIcon":"","dg-note-properties":{"title":"Conectar dos VLAN con switch y router en GNS3","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","vlan"],"status":"En progreso"}}
---


# 📝 Práctica: Conectar dos VLAN con switch y router en GNS3

> [!note] Objetivos de la Práctica
> - Crear dos VLAN repartidas entre dos switches de GNS3 unidos por un troncal dot1q.
> - Añadir un router que actúe como puerta de enlace de ambas VLAN mediante **subinterfaces**.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 4 Diseño de redes seguras/Apuntes/6. Redes virtuales (VLAN)#6.3 Enlaces troncales y etiquetado 802.1Q\|Enlaces troncales y 802.1Q]]
- [[Bastionado de redes y sistemas/Unidad 4 Diseño de redes seguras/Apuntes/6. Redes virtuales (VLAN)#6.6.1 Router-on-a-stick\|Router-on-a-stick]]

El protocolo **IEEE 802.1Q (dot1Q)** permite que varias redes compartan el mismo medio físico sin interferencias (*trunking*). Para que puertos de dos switches distintos pertenezcan a la misma VLAN, los switches deben estar unidos por un troncal.

## 🛠️ Enunciado

### Escenario

Dos aulas de un centro educativo con dos redes virtuales: profesores y alumnos.

| VLAN | Uso | Red | Puerta de enlace |
| :-: | :-: | :-: | :-: |
| 10 | Profesores | 192.168.10.0/24 | 192.168.10.254 |
| 20 | Alumnos | 192.168.20.0/24 | 192.168.20.254 |

| Equipo (VPCS) | IP | Gateway |
| :-: | :-: | :-: |
| VPCS1 | 192.168.10.1/24 | 192.168.10.254 |
| VPCS2 | 192.168.10.2/24 | 192.168.10.254 |
| VPCS3 | 192.168.20.1/24 | 192.168.20.254 |
| VPCS4 | 192.168.20.2/24 | 192.168.20.254 |

```bash
VPCS1> ip 192.168.10.1/24 192.168.10.254
```
(sintaxis `ip <dirección>/<máscara> <puerta de enlace>`; actualizado, fuente: [How to configure VPCS template preferences – GNS3 Docs](https://docs.gns3.com/docs-3.1-en/web-ui/template-preferences-vpcs))

### 1. Configurar los puertos de los switches de GNS3

En cada switch (configuración gráfica del switch integrado):

| Puerto | Tipo | VLAN |
| :-: | :-: | :-: |
| 1 | access | 10 |
| 2 | access | 20 |
| 3 | dot1q | troncal entre switches |

> [!warning] Idioma de GNS3
> Configura GNS3 **en inglés**: el tipo de puerto debe ser `access`; si el programa está en español y se escribe "acceso", dará error al conectar los hosts.

### 2. Comprobar la conectividad dentro de cada VLAN

```text
VPCS[1]> ping 192.168.10.2
VPCS[3]> ping 192.168.20.2
```

### 3. Añadir el router como puerta de enlace

En el primer switch se crea un **cuarto puerto de tipo dot1q** y se conecta a la interfaz FastEthernet 0/0 del router. En esa interfaz se crean dos subinterfaces:

```text
R1(config)#interface FastEthernet 0/0
R1(config-if)#no shut
R1(config-if)#interface FastEthernet 0/0.10
R1(config-subif)#encapsulation dot1Q 10
R1(config-subif)#ip address 192.168.10.254 255.255.255.0
R1(config-subif)#exit
R1(config)#interface FastEthernet 0/0.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip address 192.168.20.254 255.255.255.0
R1(config-subif)#end
```

### 4. Verificar

```text
VPCS[1]> ping 192.168.10.254
VPCS[3]> ping 192.168.20.254
```

Comprueba también si ahora VPCS1 alcanza a VPCS3 (enrutamiento entre VLAN a través de R1).

> [!tip] Seguridad
> El router es ahora el punto por el que pasa todo el tráfico entre VLAN: ahí se aplicarían ACL para permitir solo el tráfico necesario entre profesores y alumnos.

### ✅ Entregable

Captura de la topología, configuración de puertos de los switches, `show running-config` del router y los `ping`.

**Criterios de evaluación asociados:** RA4 b) (VLAN) y a) (enrutamiento entre segmentos).

> [!quote]- Fuentes
> - `GNS3-Conectar 2 VLAN con Switch y Router.pdf`

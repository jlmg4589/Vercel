---
{"dg-publish":true,"permalink":"/sistemas-informaticos/unidad-1-fundamentos-de-los-sistemas-informaticos/practicas/t1-ud-1-arquitecturas-microprocesador-y-ram/","title":"Arquitecturas, microprocesador y RAM","tags":["sistemas-informaticos","asignatura/sistemas-informaticos","ejercicios","arquitectura","microprocesador","memoria-ram"],"dgShowToc":true,"noteIcon":"","dg-note-properties":{"title":"Arquitecturas, microprocesador y RAM","materia":"Sistemas Informáticos","ciclo":"Formación Profesional","tipo":"practica","tema":1,"tags":["sistemas-informaticos","asignatura/sistemas-informaticos","ejercicios","arquitectura","microprocesador","memoria-ram"],"status":"En progreso"}}
---

# 📝 Tarea 1 · Arquitecturas, microprocesador y RAM

↑ [[Sistemas Informáticos/Sistemas Informáticos\|Sistemas Informáticos]] · Apuntes: [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Apuntes/1. Sistema informático y arquitectura de los ordenadores\|1. Sistema informático y arquitectura de los ordenadores]] · [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Apuntes/3. Procesador y memoria\|3. Procesador y memoria]]

> [!abstract] Resultado de aprendizaje y criterios
> **RA1**: *Evalúa sistemas informáticos, identificando sus componentes y características.* · **RA2**: *Instala sistemas operativos planificando el proceso e interpretando documentación técnica.*
> - **CE1A** (componentes físicos): ejercicios 2 y 3.
> - **CE1B** (memorias, características y prestaciones): ejercicio 4.
> - **CE2A** (elementos funcionales de un sistema informático): ejercicio 1.

> [!info] Entrega
> - Actividad de clase. Entrega un documento (PDF) con las respuestas, las **capturas** pedidas y el desarrollo de los cálculos.
> - Indica al final las **fuentes consultadas** (URL).

| Ejercicio | Criterio | Peso |
| :-: | :-: | :-: |
| 1. Arquitecturas | CE2A | 30 % |
| 2. Microprocesador del equipo | CE1A | 15 % |
| 3. Ficha oficial | CE1A | 15 % |
| 4. Memoria RAM | CE1B | 40 % |

## Ejercicio 1. Arquitecturas Von Neumann y Harvard

Busca información sobre las arquitecturas **Von Neumann** y **Harvard** y trata los siguientes aspectos:

a) Origen.
b) Funcionamiento básico, profundizando en las diferencias entre ambas.
c) Ventajas y desventajas (incluye el llamado *cuello de botella de Von Neumann*).
d) Aplicaciones: ¿qué dispositivos usan cada una? (ordenadores personales, microcontroladores, DSP…).
e) Evolución actual: explica qué es la **arquitectura Harvard modificada** y por qué las CPU actuales separan la **caché L1 de instrucciones y de datos** pero comparten la memoria principal. Comenta algún ejemplo actual (ARM, RISC-V, Apple M con memoria unificada…).

## Ejercicio 2. El microprocesador de tu equipo

Obtén, **desde el propio sistema operativo** o con una herramienta de diagnóstico, los siguientes datos del microprocesador de tu PC y adjunta una **captura**:

- Fabricante y modelo.
- Frecuencia base (y máxima, si aparece).
- Número de núcleos e hilos.
- Memoria caché (L1, L2, L3).
- Arquitectura (32 o 64 bits).

> [!tip] Herramientas
> - **Windows:** *Configuración → Sistema → Información*, `msinfo32`, el *Administrador de tareas* (pestaña *Rendimiento*) o **CPU-Z**.
> - **Linux:** `lscpu` o `cat /proc/cpuinfo`.

## Ejercicio 3. Ficha oficial del fabricante

Busca tu modelo en la web oficial del fabricante (**Intel ARK**: ark.intel.com, o las fichas de producto de **AMD**: amd.com) y completa la tabla:

| Característica | Valor |
| --- | --- |
| Núcleos / hilos | |
| Frecuencia base / turbo (boost) | |
| Caché | |
| TDP | |
| Litografía (nm) | |
| Tipo y velocidad máxima de memoria compatible | |
| Gráfica integrada | |
| Fecha de lanzamiento | |

Añade al final cualquier otra característica que consideres importante y explica por qué.

## Ejercicio 4. Cálculos sobre memoria RAM

> [!note] Recuerda
> - Los módulos se anuncian en **MT/s** (millones de transferencias por segundo), aunque muchas tiendas lo escriben como «MHz». Al ser memoria DDR, la **frecuencia real** de reloj es la mitad: $f = \text{MT/s} / 2$.
> - **Latencia real:** $t\ (\text{ns}) = \dfrac{CL \cdot 2000}{\text{MT/s}}$
> - **Ancho de banda:** $\text{MT/s} \cdot 8\ \text{bytes} \cdot \text{n.º de canales}$ (cada canal tiene un bus de 64 bits = 8 bytes).

a) Un módulo **DDR5-6000**: ¿cuál es su frecuencia real de reloj y su periodo?
b) Calcula la latencia real de un módulo **DDR4-3200 CL16** y de uno **DDR5-6000 CL30**. ¿Qué conclusión sacas?
c) ¿Qué CL debería tener un módulo **DDR5-6400** para tener una latencia real de **10 ns**?
d) Calcula el ancho de banda de dos módulos **DDR5-6000** en **single channel** y en **dual channel**.
e) ⭐ Un cliente duda entre un kit **DDR4-3600 CL18** y un kit **DDR5-5600 CL40**, ambos en dual channel. Compara la latencia real y el ancho de banda de los dos. ¿Cuál le recomendarías y para qué uso?

🡠 [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Apuntes/3. Procesador y memoria\|3. Procesador y memoria]] ⮅ [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Prácticas/T1 - Ud 1 Arquitecturas, microprocesador y RAM#📝 Tarea 1 · Arquitecturas, microprocesador y RAM\|#📝 Tarea 1 · Arquitecturas, microprocesador y RAM]] 🡢 [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Prácticas/T2 - Ud 1 Placa base\|T2 - Ud 1 Placa base]]

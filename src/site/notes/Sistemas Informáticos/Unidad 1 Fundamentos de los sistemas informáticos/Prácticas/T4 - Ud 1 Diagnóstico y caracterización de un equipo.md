---
{"dg-publish":true,"permalink":"/sistemas-informaticos/unidad-1-fundamentos-de-los-sistemas-informaticos/practicas/t4-ud-1-diagnostico-y-caracterizacion-de-un-equipo/","title":"Diagnóstico y caracterización de un equipo","tags":["sistemas-informaticos","asignatura/sistemas-informaticos","ejercicios","post","diagnostico","montaje"],"dgShowToc":true,"noteIcon":"","dg-note-properties":{"title":"Diagnóstico y caracterización de un equipo","materia":"Sistemas Informáticos","ciclo":"Formación Profesional","tipo":"practica","tema":1,"tags":["sistemas-informaticos","asignatura/sistemas-informaticos","ejercicios","post","diagnostico","montaje"],"status":"En progreso"}}
---


# 📝 Tarea 4 · Diagnóstico y caracterización de un equipo

↑ [[Sistemas Informáticos/Sistemas Informáticos\|Sistemas Informáticos]] · Apuntes: [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Apuntes/5. Puesta en marcha del equipo. POST\|5. Puesta en marcha del equipo. POST]] · [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Apuntes/4. Almacenamiento, alimentación, expansión y periféricos\|4. Almacenamiento, alimentación, expansión y periféricos]] · [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Apuntes/7. Normas de seguridad y prevención de riesgos laborales\|7. Normas de seguridad y prevención de riesgos laborales]]

> [!abstract] Resultado de aprendizaje y criterios
> **RA1**: *Evalúa sistemas informáticos, identificando sus componentes y características.* · **RA2** (parcial).
> - **CE1A** (componentes físicos): ejercicio 7.
> - **CE1B** (memorias): ejercicios 1, 2 y 5.
> - **CE1C** (puesta en marcha y POST): ejercicios 1 a 5.
> - **CE1D** (instalación y configuración de periféricos): ejercicio 6.
> - **CE1H** (seguridad y prevención de riesgos laborales): ejercicio 7.
> - **CE2A** (elementos funcionales): ejercicio 7.

> [!info] Tipo de actividad
> Tarea **con entrega**.

## Instrucciones (ejercicios 1 a 5)

Para cada caso:

1. Lee los síntomas y los datos del POST.
2. Usa un motor de búsqueda para encontrar el **manual de la placa base** o la **lista de códigos** del fabricante de la BIOS.
3. Determina la **causa más probable** del problema.
4. Propón una **acción de reparación**.
5. Indica la **fuente** consultada (URL).

## Ejercicio 1. El PC «mudo»

- **Fabricante/BIOS:** placa base genérica con BIOS **AMI** (American Megatrends).
- **Síntomas:** todos los ventiladores (CPU, fuente, caja) giran, pero el monitor se queda completamente negro. No se oye el inicio de Windows.
- **Datos del POST:** un pitido largo seguido de tres cortos (*beeep… beep-beep-beep*).
- **Tarea:** ¿qué significa este código de pitidos en una BIOS AMI y qué componente falla?

## Ejercicio 2. El display «55»

- **Placa:** **ASUS ROG Strix Z690-F**.
- **Síntomas:** un alumno acaba de montar su PC nuevo. Al encenderlo se activan las luces RGB y giran los ventiladores, pero no hay señal de vídeo.
- **Datos del POST:** el display de 2 dígitos (**Q-Code**) se queda fijo en **«55»**.
- **Tarea:** busca el manual de la placa o la tabla de Q-Codes de ASUS. ¿Qué indica el código «55» y cuál es su causa más común?

## Ejercicio 3. El fallo gráfico clásico

- **Fabricante/BIOS:** placa base antigua con BIOS **Award** (o Phoenix-Award).
- **Síntomas:** el ordenador enciende y se oye actividad del disco duro (como si cargara el sistema operativo), pero la pantalla permanece negra desde el primer segundo.
- **Datos del POST:** un pitido largo seguido de dos cortos (*beeeeep… beep-beep*).
- **Tarea:** investiga los códigos de pitidos de la BIOS Award. ¿Qué componente esencial está fallando?

## Ejercicio 4. El LED de la CPU

- **Placa:** **MSI MAG B760 TOMAHAWK WIFI**.
- **Síntomas:** al pulsar el botón de encendido, los ventiladores intentan girar medio segundo y el sistema se apaga. En un segundo intento el PC se queda encendido pero «muerto» (sin vídeo ni actividad).
- **Datos del POST:** estas placas tienen **EZ Debug LEDs**. En lugar de un código numérico, se queda encendida una **luz roja fija** junto a la etiqueta **«CPU»**.
- **Tarea:** ¿qué significa el EZ Debug LED de CPU en las placas MSI? ¿Cuáles son las 2 o 3 causas más probables?

## Ejercicio 5. El código «C1»

- **Placa:** **Gigabyte Z790 AORUS ELITE**.
- **Síntomas:** el PC funcionaba bien. Tras una limpieza de polvo, al volver a encenderlo se queda atascado durante el arranque, sin vídeo.
- **Datos del POST:** el display LED de 2 dígitos se queda congelado en **«C1»**.
- **Tarea:** busca la lista de *POST codes* de las placas Gigabyte/AORUS. ¿A qué componente se refiere el código «C1» y qué debería revisar el usuario tras la limpieza?
<!--

-->
## Ejercicio 6. Instalación y configuración de un periférico

Elige un periférico disponible en el aula (impresora, escáner, webcam, tableta gráfica…) e:

1. **Clasifícalo**: de entrada, de salida o de entrada/salida, e indica su tipo de conexión.
2. **Instálalo**: localiza y descarga el **controlador** en la web oficial del fabricante, o comprueba que el sistema operativo lo reconoce automáticamente.
3. **Configúralo**: ajusta al menos dos parámetros (resolución, calidad, idioma, configuración predeterminada…).
4. **Comprueba su funcionamiento** y documenta el proceso con capturas.

## Ejercicio 7. Caracterización de un equipo informático

En grupos designados por el profesor:

1. Antes de empezar, elabora una **lista de comprobación de seguridad** y aplícala: equipo apagado y desenchufado, pulsera o alfombrilla antiestática, superficie adecuada, manipulación de componentes por sus bordes, orden de los tornillos…
2. **Desmonta** el equipo e **identifica todos sus componentes**, fotografiando cada una de las partes estudiadas en el tema.
3. Indica para cada componente su **función** dentro del sistema.
4. Enumera los **pasos de montaje** y vuelve a montar el equipo.
5. Comprueba que el equipo **arranca y supera el POST**.

🡠 [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Prácticas/T3 - Ud 1 Presupuesto de equipo informático\|T3 - Ud 1 Presupuesto de equipo informático]] ⮅ [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Prácticas/T4 - Ud 1 Diagnóstico y caracterización de un equipo#📝 Tarea 4 · Diagnóstico y caracterización de un equipo\|#📝 Tarea 4 · Diagnóstico y caracterización de un equipo]] 🡢 [[Sistemas Informáticos/Unidad 1 Fundamentos de los sistemas informáticos/Apuntes/5. Puesta en marcha del equipo. POST\|5. Puesta en marcha del equipo. POST]]

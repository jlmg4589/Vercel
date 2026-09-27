---
{"dg-publish":true,"permalink":"/apuntes/sistemas-informaticos/01-temario/tema-00-sistemas-de-numeracion/fe-de-erratas-tema-00/","tags":["asignatura/sistemas-informaticos","tema/00","erratas"],"dg-note-properties":{"tipo":"erratas","asignatura":"Sistemas Informáticos","tema":0,"fuente":"[[Sistemas de numeración.pdf]]","tags":["asignatura/sistemas-informaticos","tema/00","erratas"]}}
---


# Fe de erratas - Tema 00

↑ [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/Tema 00 - Sistemas de numeración\|Tema 00 - Sistemas de numeración]]

Errores encontrados en las diapositivas de [Sistemas de numeración.pdf](/img/user/Apuntes/Sistemas%20Inform%C3%A1ticos/Sistemas%20de%20numeraci%C3%B3n.pdf) y ya corregidos en los apuntes.

## Errores de contenido

| Diap. | Dónde | En la diapositiva | Corrección | Nota |
| :-: | --- | --- | --- | --- |
| 11 | Binario → decimal | $11{,}011_2 = \dots = 1+2{,}0+1/4+1/8 = 3{,}3/8$ | $= 2+1+0+0{,}25+0{,}125 = 3{,}375_{10}$ | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/1. Representación de la información/1.3 Cambios de base\|1.3 Cambios de base]] |
| 12 | Decimal → binario | "dividir entre 2 hasta que el **resto** sea menor que 2" | hasta que el **cociente** sea menor que 2 | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/1. Representación de la información/1.3 Cambios de base\|1.3 Cambios de base]] |
| 17 | Decimal → hexadecimal, ej. 1 | $29_{10} = 1D_8$ | $29_{10} = 1D_{16}$ | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/1. Representación de la información/1.3 Cambios de base\|1.3 Cambios de base]] |
| 20 | Tabla de la resta | Columna "C (Acarreo)" | Es un **préstamo** (*borrow*) | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/1. Representación de la información/1.4.1 Operaciones aritméticas\|1.4.1 Operaciones aritméticas]] |
| 23 | Fórmula XOR | `(NOT(A) AND B) OR (A AND (NOT (B))` (falta un paréntesis) | $(\text{NOT}(A) \text{ AND } B) \text{ OR } (A \text{ AND } \text{NOT}(B))$ | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/1. Representación de la información/1.4.2 Operaciones lógicas\|1.4.2 Operaciones lógicas]] |
| 29 | Complemento a 1 | "Se obtiene al cambiar los 0 por 1" | …los 0 por 1 **y los 1 por 0** | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/1. Representación de la información/1.4.3 Complementos\|1.4.3 Complementos]] |
| 35 | Paridad bidimensional | El enunciado ya muestra los bits de paridad (4 filas de 8 bits) | Datos: 3 filas de 7 bits; la 8.ª columna y la 4.ª fila son la solución | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/1. Representación de la información/1.5 Detección de errores\|1.5 Detección de errores]] |
| 40 | Tabla de múltiplos | gigabyte **(MB)** | gigabyte **(GB)** | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/2. Codificación de la información/2.1 Almacenamiento de la información\|2.1 Almacenamiento de la información]] |
| 43 | Decimal empaquetado | "representa… en decimal **desempaquetado**" | …en decimal **empaquetado** | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/2. Codificación de la información/2.2.1 Números enteros\|2.2.1 Números enteros]] |
| 46 | Coma flotante | "El exponente y la mantisa son números enteros" | El exponente es entero; la mantisa es, en general, fraccionaria (en IEEE 754, $1{,}xxx$) | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/2. Codificación de la información/2.2.2 Números con decimales\|2.2.2 Números con decimales]] |
| 47 | Notación científica | "todos los dígitos a la derecha del punto decimal y a la izquierda siempre un dígito distinto de 0" (contradictorio) | A la izquierda de la coma, **un único** dígito distinto de 0; el resto a la derecha | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/2. Codificación de la información/2.2.2 Números con decimales\|2.2.2 Números con decimales]] |
| 50 | IEEE 754, ejemplo 1 | Mantisa `10000001001100000000000`; dato `01000010 01000001 00110000 00000000` | Mantisa `10000001000000000000000`; dato `01000010 01000000 10000000 00000000` ($42408000_{16}$) | [[Apuntes/Sistemas Informáticos/01_Temario/Tema 00 - Sistemas de numeración/2. Codificación de la información/2.2.2 Números con decimales\|2.2.2 Números con decimales]] |

## Erratas tipográficas corregidas

- "dígito binario o **bits**" → *bit* (diap. 2)
- "parte **fraccionariadel**" → fraccionaria del; "son **cada los** dígitos" → cada uno de los dígitos (diap. 5)
- "**nuemración**" → numeración (diap. 7)
- "sistema **decima**" → decimal (diap. 18)
- "pero en realidad lo que realizan solo con operaciones lógicas" → "en realidad solo realizan operaciones lógicas" (diap. 21)
- "Se añade **un bits**" → un bit (diap. 34)
- "**internamiente**", "**represntar**", "**Represemta** los **soguientes**", "se **pude** representar", "simple **recisión**" (diap. 41–50)

## Comprobado y correcto

Se verificaron numéricamente todos los demás ejemplos: cambios de base ($1342_8$, $34{,}03_8$, $335_8$, $21{,}44_8$, $3AF_{16}$, $F0{,}0C_{16}$, $1B2_{16}$, $2B{,}44_{16}$, $35_8$, $F12A4_{16} = 987812$, $7B4_{16}$), sumas y restas binarias, tablas de verdad y ejemplos lógicos, complementos, CRC-3 (resto `011`), binario con signo y el ejemplo 2 de IEEE 754.

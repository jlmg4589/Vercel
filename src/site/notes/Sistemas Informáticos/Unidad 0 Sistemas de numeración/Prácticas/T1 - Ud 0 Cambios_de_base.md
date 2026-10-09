---
{"dg-publish":true,"permalink":"/sistemas-informaticos/unidad-0-sistemas-de-numeracion/practicas/t1-ud-0-cambios-de-base/","title":"Cambios de base","tags":["sistemas-informaticos","asignatura/sistemas-informaticos","sistemas-numeracion","conversiones","ejercicios","binario","octal","hexadecimal","tfn"],"dgShowToc":true,"noteIcon":"","dg-note-properties":{"title":"Cambios de base","materia":"Sistemas Informáticos","ciclo":"Formación Profesional","tipo":"practica","tema":0,"tags":["sistemas-informaticos","asignatura/sistemas-informaticos","sistemas-numeracion","conversiones","ejercicios","binario","octal","hexadecimal","tfn"],"status":"En progreso"}}
---


# 📝 Práctica de repaso: cambios de base · Unidad 0

↑ [[Sistemas Informáticos/Sistemas Informáticos\|Sistemas Informáticos]] · Apuntes: [[Sistemas Informáticos/Unidad 0 Sistemas de numeración/Apuntes/1. Representación de la información\|1. Representación de la información]]

> [!note] Objetivos de la práctica
> - Dominar la conversión de cualquier base a decimal aplicando el Teorema Fundamental de la Numeración (TFN).
> - Manejar la conversión directa entre las bases $8$ y $16$ utilizando el sistema binario como paso intermedio mediante agrupación de bits.

> [!abstract] Resultado de aprendizaje y criterios
> Práctica de la unidad **introductoria** (sin criterios propios). Da soporte al **RA1**: *Evalúa sistemas informáticos identificando sus componentes y características*, en especial a **CE1A** (representación binaria de la información en los componentes físicos) y prepara el manejo de binario y hexadecimal necesario para **CE1E** y la UD5 (direcciones IP y MAC).

## 📐 Recordatorio teórico

> [!tip] Teorema Fundamental de la Numeración (TFN)
> Un número escrito en una base $b$ con dígitos $d_i$ equivale en base decimal a la suma ponderada del valor de sus dígitos multiplicados por la base elevada a la posición que ocupan:
> $$N_{10} = \sum_{i=0}^{n-1} d_i \cdot b^i = d_{n-1} \cdot b^{n-1} + \dots + d_1 \cdot b^1 + d_0 \cdot b^0$$

> [!example] Equivalencia de agrupación de bits
> - Base octal ($b=8=2^3$): cada dígito octal equivale exactamente a 3 bits.
> - Base hexadecimal ($b=16=2^4$): cada dígito hexadecimal equivale exactamente a 4 bits.

> [!info] Soluciones
> Las soluciones están **plegadas** al final de cada bloque: intenta resolverlo antes de desplegarlas.

<div style="page-break-after: always;"></div>

## Parte 1: Conversión a base decimal (TFN)

Aplica el Teorema Fundamental de la Numeración (desarrollo polinómico) para convertir las siguientes cantidades a su equivalente en base $10$. Muestra el desglose completo.

### Base 2 (binario)

1. $(10110)_2$
2. $(1101101)_2$
3. $(111000)_2$
4. $(11111)_2$
5. $(1010101)_2$

> [!success]- Solución (base 2)
> 1. $1 \cdot 2^4 + 0 \cdot 2^3 + 1 \cdot 2^2 + 1 \cdot 2^1 + 0 \cdot 2^0 = 16 + 0 + 4 + 2 + 0 = (22)_{10}$
> 2. $1 \cdot 2^6 + 1 \cdot 2^5 + 0 \cdot 2^4 + 1 \cdot 2^3 + 1 \cdot 2^2 + 0 \cdot 2^1 + 1 \cdot 2^0 = 64 + 32 + 0 + 8 + 4 + 0 + 1 = (109)_{10}$
> 3. $1 \cdot 2^5 + 1 \cdot 2^4 + 1 \cdot 2^3 + 0 + 0 + 0 = 32 + 16 + 8 = (56)_{10}$
> 4. $1 \cdot 2^4 + 1 \cdot 2^3 + 1 \cdot 2^2 + 1 \cdot 2^1 + 1 \cdot 2^0 = 16 + 8 + 4 + 2 + 1 = (31)_{10}$
> 5. $1 \cdot 2^6 + 0 \cdot 2^5 + 1 \cdot 2^4 + 0 \cdot 2^3 + 1 \cdot 2^2 + 0 \cdot 2^1 + 1 \cdot 2^0 = 64 + 0 + 16 + 0 + 4 + 0 + 1 = (85)_{10}$

### Base 8 (octal)

6. $(345)_8$
7. $(702)_8$
8. $(156)_8$
9. $(43)_8$
10. $(107)_8$

> [!success]- Solución (base 8)
> 6. $3 \cdot 8^2 + 4 \cdot 8^1 + 5 \cdot 8^0 = 192 + 32 + 5 = (229)_{10}$
> 7. $7 \cdot 8^2 + 0 \cdot 8^1 + 2 \cdot 8^0 = 448 + 0 + 2 = (450)_{10}$
> 8. $1 \cdot 8^2 + 5 \cdot 8^1 + 6 \cdot 8^0 = 64 + 40 + 6 = (110)_{10}$
> 9. $4 \cdot 8^1 + 3 \cdot 8^0 = 32 + 3 = (35)_{10}$
> 10. $1 \cdot 8^2 + 0 \cdot 8^1 + 7 \cdot 8^0 = 64 + 0 + 7 = (71)_{10}$

### Base 16 (hexadecimal)

11. $(A4)_{16}$
12. $(1F3)_{16}$
13. $(C0B)_{16}$
14. $(8D)_{16}$
15. $(E2)_{16}$

> [!success]- Solución (base 16)
> 11. $10 \cdot 16^1 + 4 \cdot 16^0 = 160 + 4 = (164)_{10}$
> 12. $1 \cdot 16^2 + 15 \cdot 16^1 + 3 \cdot 16^0 = 256 + 240 + 3 = (499)_{10}$
> 13. $12 \cdot 16^2 + 0 \cdot 16^1 + 11 \cdot 16^0 = 3072 + 0 + 11 = (3083)_{10}$
> 14. $8 \cdot 16^1 + 13 \cdot 16^0 = 128 + 13 = (141)_{10}$
> 15. $14 \cdot 16^1 + 2 \cdot 16^0 = 224 + 2 = (226)_{10}$

### Otras bases

16. $(210)_3$
17. $(1022)_3$
18. $(312)_4$
19. $(404)_5$
20. $(61)_7$

> [!success]- Solución (otras bases)
> 16. $2 \cdot 3^2 + 1 \cdot 3^1 + 0 \cdot 3^0 = 18 + 3 + 0 = (21)_{10}$
> 17. $1 \cdot 3^3 + 0 \cdot 3^2 + 2 \cdot 3^1 + 2 \cdot 3^0 = 27 + 0 + 6 + 2 = (35)_{10}$
> 18. $3 \cdot 4^2 + 1 \cdot 4^1 + 2 \cdot 4^0 = 48 + 4 + 2 = (54)_{10}$
> 19. $4 \cdot 5^2 + 0 \cdot 5^1 + 4 \cdot 5^0 = 100 + 0 + 4 = (104)_{10}$
> 20. $6 \cdot 7^1 + 1 \cdot 7^0 = 42 + 1 = (43)_{10}$

<div style="page-break-after: always;"></div>

## Parte 2: Conversión entre las bases octal y hexadecimal

Realiza los cambios de base pasando primero a binario (agrupando en bloques de 3 bits para octal y de 4 bits para hexadecimal).

### De octal a hexadecimal

1. $(74)_8$
2. $(153)_8$
3. $(420)_8$
4. $(671)_8$
5. $(235)_8$
6. $(506)_8$
7. $(112)_8$
8. $(377)_8$
9. $(1054)_8$
10. $(726)_8$

> [!success]- Solución (octal → hexadecimal)
> 1. $(74)_8 = 111\ 100_2 = 0011\ 1100_2 = (3C)_{16}$
> 2. $(153)_8 = 001\ 101\ 011_2 = 0110\ 1011_2 = (6B)_{16}$
> 3. $(420)_8 = 100\ 010\ 000_2 = 0001\ 0001\ 0000_2 = (110)_{16}$
> 4. $(671)_8 = 110\ 111\ 001_2 = 0001\ 1011\ 1001_2 = (1B9)_{16}$
> 5. $(235)_8 = 010\ 011\ 101_2 = 1001\ 1101_2 = (9D)_{16}$
> 6. $(506)_8 = 101\ 000\ 110_2 = 0001\ 0100\ 0110_2 = (146)_{16}$
> 7. $(112)_8 = 001\ 001\ 010_2 = 0100\ 1010_2 = (4A)_{16}$
> 8. $(377)_8 = 011\ 111\ 111_2 = 1111\ 1111_2 = (FF)_{16}$
> 9. $(1054)_8 = 001\ 000\ 101\ 100_2 = 0010\ 0010\ 1100_2 = (22C)_{16}$
> 10. $(726)_8 = 111\ 010\ 110_2 = 0001\ 1101\ 0110_2 = (1D6)_{16}$

### De hexadecimal a octal

11. $(3A)_{16}$
12. $(F2)_{16}$
13. $(1C4)_{16}$
14. $(8B)_{16}$
15. $(D7)_{16}$
16. $(2E0)_{16}$
17. $(9F)_{16}$
18. $(5C)_{16}$
19. $(A1B)_{16}$
20. $(E4)_{16}$

> [!success]- Solución (hexadecimal → octal)
> 11. $(3A)_{16} = 0011\ 1010_2 = 111\ 010_2 = (72)_8$
> 12. $(F2)_{16} = 1111\ 0010_2 = 011\ 110\ 010_2 = (362)_8$
> 13. $(1C4)_{16} = 0001\ 1100\ 0100_2 = 000\ 111\ 000\ 100_2 = (704)_8$
> 14. $(8B)_{16} = 1000\ 1011_2 = 010\ 001\ 011_2 = (213)_8$
> 15. $(D7)_{16} = 1101\ 0111_2 = 011\ 010\ 111_2 = (327)_8$
> 16. $(2E0)_{16} = 0010\ 1110\ 0000_2 = 001\ 011\ 100\ 000_2 = (1340)_8$
> 17. $(9F)_{16} = 1001\ 1111_2 = 010\ 011\ 111_2 = (237)_8$
> 18. $(5C)_{16} = 0101\ 1100_2 = 001\ 011\ 100_2 = (134)_8$
> 19. $(A1B)_{16} = 1010\ 0001\ 1011_2 = 101\ 000\ 011\ 011_2 = (5033)_8$
> 20. $(E4)_{16} = 1110\ 0100_2 = 011\ 100\ 100_2 = (344)_8$

🡠 [[Sistemas Informáticos/Unidad 0 Sistemas de numeración/Apuntes/1. Representación de la información\|1. Representación de la información]] ⮅ [[Sistemas Informáticos/Unidad 0 Sistemas de numeración/Prácticas/T1 - Ud 0 Cambios_de_base#📝 Práctica de repaso: cambios de base · Unidad 0\|#📝 Práctica de repaso: cambios de base · Unidad 0]] 🡢 [[Sistemas Informáticos/Unidad 0 Sistemas de numeración/Prácticas/T2 - Ud 0 Relación de ejercicios preparatorios\|T2 - Ud 0 Relación de ejercicios preparatorios]]

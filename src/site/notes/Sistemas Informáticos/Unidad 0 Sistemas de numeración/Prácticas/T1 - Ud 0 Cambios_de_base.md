---
{"dg-publish":true,"permalink":"/sistemas-informaticos/unidad-0-sistemas-de-numeracion/practicas/t1-ud-0-cambios-de-base/","title":"Cambios de base","tags":["sistemas-informaticos","conversiones","ejercicios","binario","octal","hexadecimal","tfn"],"noteIcon":"","dg-note-properties":{"title":"Cambios de base","materia":"Sistemas Informáticos","ciclo":"Formación Profesional","tags":["sistemas-informaticos","conversiones","ejercicios","binario","octal","hexadecimal","tfn"],"status":"En progreso"}}
---

---

📝 Práctica de Repaso: Sistemas de Numeración

> [!note] Objetivos de la Práctica
> Dominar la conversión de cualquier base a decimal aplicando el Teorema Fundamental de la Numeración (TFN).
> Manejar la conversión directa entre bases $8$ y $16$ utilizando el sistema binario como paso intermedio mediante agrupación de bits.

📐 Recordatorio Teórico

> [!tip] Teorema Fundamental de la Numeración (TFN)
> Un número escrito en una base $b$ con dígitos $d_i$ equivale en base decimal a la suma ponderada del valor de sus dígitos multiplicados por la base elevada a la posición que ocupan:
> $$N_{10} = \sum_{i=0}^{n-1} d_i \cdot b^i = d_{n-1} \cdot b^{n-1} + \dots + d_1 \cdot b^1 + d_0 \cdot b^0$$

> [!example] Equivalencia de agrupación de bits
> Base Octal ($b=8=2^3$): Cada dígito octal equivale exactamente a 3 bits.
> Base Hexadecimal ($b=16=2^4$): Cada dígito hexadecimal equivale exactamente a 4 bits.

<div style="page-break-after: always;"></div>

---
 

### Parte 1: Conversión a base decimal (TFN)

Aplica el Teorema Fundamental de la Numeración (desarrollo polinómico) para convertir las siguientes cantidades a su equivalente en base $10$. Muestra el desglose completo.

#### Ejercicios en Base 2 (Binario)
1. $(10110)_2$: <span style="color: white;">$1 \cdot 2^4 + 0 \cdot 2^3 + 1 \cdot 2^2 + 1 \cdot 2^1 + 0 \cdot 2^0 = 16 + 0 + 4 + 2 + 0 = (22)_{10}$</span>
2. $(1101101)_2$: <span style="color: white;">$1 \cdot 2^6 + 1 \cdot 2^5 + 0 \cdot 2^4 + 1 \cdot 2^3 + 1 \cdot 2^2 + 0 \cdot 2^1 + 1 \cdot 2^0 = 64 + 32 + 0 + 8 + 4 + 0 + 1 = (109)_{10}$</span>
3. $(111000)_2$: <span style="color: white;">$1 \cdot 2^5 + 1 \cdot 2^4 + 1 \cdot 2^3 + 0 + 0 + 0 = 32 + 16 + 8 = (56)_{10}$</span>
4. $(11111)_2$: <span style="color: white;">$1 \cdot 2^4 + 1 \cdot 2^3 + 1 \cdot 2^2 + 1 \cdot 2^1 + 1 \cdot 2^0 = 16 + 8 + 4 + 2 + 1 = (31)_{10}$</span>
5. $(1010101)_2$: <span style="color: white;">$1 \cdot 2^6 + 0 \cdot 2^5 + 1 \cdot 2^4 + 0 \cdot 2^3 + 1 \cdot 2^2 + 0 \cdot 2^1 + 1 \cdot 2^0 = 64 + 0 + 16 + 0 + 4 + 0 + 1 = (85)_{10}$</span>

#### Ejercicios en Base 8 (Octal)
6. $(345)_8$: <span style="color: white;">$3 \cdot 8^2 + 4 \cdot 8^1 + 5 \cdot 8^0 = 3 \cdot 64 + 4 \cdot 8 + 5 \cdot 1 = 192 + 32 + 5 = (229)_{10}$</span>
7. $(702)_8$: <span style="color: white;">$7 \cdot 8^2 + 0 \cdot 8^1 + 2 \cdot 8^0 = 7 \cdot 64 + 0 + 2 = 448 + 2 = (450)_{10}$</span>
8. $(156)_8$: <span style="color: white;">$1 \cdot 8^2 + 5 \cdot 8^1 + 6 \cdot 8^0 = 64 + 40 + 6 = (110)_{10}$</span>
9. $(43)_8$: <span style="color: white;">$4 \cdot 8^1 + 3 \cdot 8^0 = 32 + 3 = (35)_{10}$</span>
10. $(107)_8$: <span style="color: white;">$1 \cdot 8^2 + 0 \cdot 8^1 + 7 \cdot 8^0 = 64 + 0 + 7 = (71)_{10}$</span>

#### Ejercicios en Base 16 (Hexadecimal)
11. $(A4)_{16}$: <span style="color: white;">$10 \cdot 16^1 + 4 \cdot 16^0 = 160 + 4 = (164)_{10}$</span>
12. $(1F3)_{16}$: <span style="color: white;">$1 \cdot 16^2 + 15 \cdot 16^1 + 3 \cdot 16^0 = 256 + 240 + 3 = (499)_{10}$</span>
13. $(C0B)_{16}$: <span style="color: white;">$12 \cdot 16^2 + 0 \cdot 16^1 + 11 \cdot 16^0 = 12 \cdot 256 + 0 + 11 = 3072 + 11 = (3083)_{10}$</span>
14. $(8D)_{16}$: <span style="color: white;">$8 \cdot 16^1 + 13 \cdot 16^0 = 128 + 13 = (141)_{10}$</span>
15. $(E2)_{16}$: <span style="color: white;">$14 \cdot 16^1 + 2 \cdot 16^0 = 224 + 2 = (226)_{10}$</span>

#### Ejercicios en Otras Bases
16. $(210)_3$: <span style="color: white;">$2 \cdot 3^2 + 1 \cdot 3^1 + 0 \cdot 3^0 = 18 + 3 + 0 = (21)_{10}$</span>
17. $(1022)_3$: <span style="color: white;">$1 \cdot 3^3 + 0 \cdot 3^2 + 2 \cdot 3^1 + 2 \cdot 3^0 = 27 + 0 + 6 + 2 = (35)_{10}$</span>
18. $(312)_4$: <span style="color: white;">$3 \cdot 4^2 + 1 \cdot 4^1 + 2 \cdot 4^0 = 48 + 4 + 2 = (54)_{10}$</span>
19. $(404)_5$: <span style="color: white;">$4 \cdot 5^2 + 0 \cdot 5^1 + 4 \cdot 5^0 = 100 + 0 + 4 = (104)_{10}$</span>
20. $(61)_7$: <span style="color: white;">$6 \cdot 7^1 + 1 \cdot 7^0 = 42 + 1 = (43)_{10}$</span>

<div style="page-break-after: always;"></div>

---

### Parte 2: Conversión entre bases Octal y Hexadecimal

Realiza los cambios de base pasando primero a binario (agrupando en bloques de 3 bits para octal y 4 bits para hexadecimal).

#### De base Octal a base Hexadecimal
1. $(74)_8$: <span style="color: white;">$(74)_8 = 111\ 100_2 = 0011\ 1100_2 = (3C)_{16}$</span>
2. $(153)_8$: <span style="color: white;">$(153)_8 = 001\ 101\ 011_2 = 0110\ 1011_2 = (6B)_{16}$</span>
3. $(420)_8$: <span style="color: white;">$(420)_8 = 100\ 010\ 000_2 = 0001\ 0001\ 0000_2 = (110)_{16}$</span>
4. $(671)_8$: <span style="color: white;">$(671)_8 = 110\ 111\ 001_2 = 0001\ 1011\ 1001_2 = (1B9)_{16}$</span>
5. $(235)_8$: <span style="color: white;">$(235)_8 = 010\ 011\ 101_2 = 1001\ 1101_2 = (9D)_{16}$</span>
6. $(506)_8$: <span style="color: white;">$(506)_8 = 101\ 000\ 110_2 = 0001\ 0100\ 0110_2 = (146)_{16}$</span>
7. $(112)_8$: <span style="color: white;">$(112)_8 = 001\ 001\ 010_2 = 0100\ 1010_2 = (4A)_{16}$</span>
8. $(377)_8$: <span style="color: white;">$(377)_8 = 011\ 111\ 111_2 = 1111\ 1111_2 = (FF)_{16}$</span>
9. $(1054)_8$: <span style="color: white;">$(1054)_8 = 001\ 000\ 101\ 100_2 = 0010\ 0010\ 1100_2 = (22C)_{16}$</span>
10. $(726)_8$: <span style="color: white;">$(726)_8 = 111\ 010\ 110_2 = 0001\ 1101\ 0110_2 = (1D6)_{16}$</span>

#### De base Hexadecimal a base Octal
11. $(3A)_{16}$: <span style="color: white;">$(3A)_{16} = 0011\ 1010_2 = 111\ 010_2 = (72)_8$</span>
12. $(F2)_{16}$: <span style="color: white;">$(F2)_{16} = 1111\ 0010_2 = 011\ 110\ 010_2 = (362)_8$</span>
13. $(1C4)_{16}$: <span style="color: white;">$(1C4)_{16} = 0001\ 1100\ 0100_2 = 000\ 111\ 000\ 100_2 = (704)_8$</span>
14. $(8B)_{16}$: <span style="color: white;">$(8B)_{16} = 1000\ 1011_2 = 010\ 001\ 011_2 = (213)_8$</span>
15. $(D7)_{16}$: <span style="color: white;">$(D7)_{16} = 1101\ 0111_2 = 011\ 010\ 111_2 = (327)_8$</span>
16. $(2E0)_{16}$: <span style="color: white;">$(2E0)_{16} = 0010\ 1110\ 0000_2 = 001\ 011\ 100\ 000_2 = (1340)_8$</span>
17. $(9F)_{16}$: <span style="color: white;">$(9F)_{16} = 1001\ 1111_2 = 010\ 011\ 111_2 = (237)_8$</span>
18. $(5C)_{16}$: <span style="color: white;">$(5C)_{16} = 0101\ 1100_2 = 001\ 011\ 100_2 = (134)_8$</span>
19. $(A1B)_{16}$: <span style="color: white;">$(A1B)_{16} = 1010\ 0001\ 1011_2 = 101\ 000\ 011\ 011_2 = (5033)_8$</span>
20. $(E4)_{16}$: <span style="color: white;">$(E4)_{16} = 1110\ 0100_2 = 011\ 100\ 100_2 = (344)_8$</span>


> [!info] Soluciones
> Las soluciones de cada apartado están escritas en blanco a continuación de cada ejercicio, si desea comprobar su solución, solo tiene que sombrearla o copiarla y pegarla en un editor.
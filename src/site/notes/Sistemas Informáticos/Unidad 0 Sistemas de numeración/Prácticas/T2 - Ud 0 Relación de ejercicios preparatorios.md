---
{"dg-publish":true,"permalink":"/sistemas-informaticos/unidad-0-sistemas-de-numeracion/practicas/t2-ud-0-relacion-de-ejercicios-preparatorios/","title":"Relación de ejercicios preparatorios","tags":["sistemas-informaticos","asignatura/sistemas-informaticos","ejercicios","sistemas-numeracion","codificacion"],"dgShowToc":true,"noteIcon":"","dg-note-properties":{"title":"Relación de ejercicios preparatorios","materia":"Sistemas Informáticos","ciclo":"Formación Profesional","tipo":"practica","tema":0,"tags":["sistemas-informaticos","asignatura/sistemas-informaticos","ejercicios","sistemas-numeracion","codificacion"],"status":"Publicado"}}
---


# 📝 Relación de ejercicios preparatorios · Unidad 0

↑ [[Sistemas Informáticos/Sistemas Informáticos\|Sistemas Informáticos]] · Apuntes: [[Sistemas Informáticos/Unidad 0 Sistemas de numeración/Apuntes/1. Representación de la información\|1. Representación de la información]] · [[Sistemas Informáticos/Unidad 0 Sistemas de numeración/Apuntes/2. Codificación de la información\|2. Codificación de la información]]

> [!note] Objetivos
> Repasar **todo** el contenido de la Unidad 0 antes del examen: sistemas de numeración, cambios de base, operaciones aritméticas y lógicas en binario, complementos, detección de errores (paridad y CRC) y codificación de la información (unidades, BCD, decimal empaquetado y desempaquetado, signo-magnitud e IEEE 754).
>
> Los ejercicios van de **menor a mayor dificultad** dentro de cada bloque. Las soluciones están plegadas: intenta resolver cada bloque antes de desplegarlas.

> [!tip] Cómo usar esta relación
> - En el examen se puede usar **calculadora**, pero practica también **a mano**: muchas preguntas piden el procedimiento (complementos, BCD, IEEE 754, CRC) y la calculadora no lo hace por ti.
> - Trabaja siempre con el **número de bits** que indique el enunciado (8 bits salvo que se diga otra cosa) y conserva los ceros a la izquierda.
> - Los ejercicios marcados con ⭐ son de nivel alto (equivalentes a las preguntas para sacar más de un 8).

## 1. Conceptos básicos

**1.1** Clasifica cada dato según el lugar que ocupa en el proceso (entrada, interno del proceso o salida):
a) La nota media que muestra una aplicación en pantalla. b) El número que tecleas en una calculadora. c) Un resultado parcial que guarda el procesador antes de mostrar el total. d) El sonido que reproduce un altavoz.

**1.2** Indica si los siguientes sistemas son posicionales o no posicionales: decimal, números romanos, binario, hexadecimal.

**1.3** Escribe el desarrollo polinómico (TFN en base 10) de: a) $4072$ b) $305{,}28$ c) $0{,}906$

**1.4** ¿Cuántos símbolos distintos usa cada sistema? Escribe sus símbolos: base 2, base 5, base 8, base 16.

**1.5** ¿Cuáles de estos números **no** son válidos en la base indicada? Justifícalo: $1021_2$, $758_8$, $1G4_{16}$, $4320_5$, $FACE_{16}$.

**1.6** ¿Cuántos valores distintos se pueden representar con 4, 8, 10 y 16 bits? ¿Cuántos bits harían falta como mínimo para representar 300 valores distintos?

> [!success]- Solución bloque 1
> **1.1** a) salida · b) entrada · c) interno del proceso · d) salida.
> **1.2** Decimal, binario y hexadecimal son **posicionales** (el valor depende de la posición); los romanos **no posicionales**.
> **1.3** a) $4\cdot10^3+0\cdot10^2+7\cdot10^1+2\cdot10^0$ · b) $3\cdot10^2+0\cdot10^1+5\cdot10^0+2\cdot10^{-1}+8\cdot10^{-2}$ · c) $0\cdot10^0+9\cdot10^{-1}+0\cdot10^{-2}+6\cdot10^{-3}$
> **1.4** Base 2: 2 símbolos {0,1} · base 5: 5 {0–4} · base 8: 8 {0–7} · base 16: 16 {0–9, A–F}.
> **1.5** No válidos: $1021_2$ (el 2 no existe en binario), $758_8$ (el 8 no existe en octal), $1G4_{16}$ (G no es dígito hexadecimal). Válidos: $4320_5$ y $FACE_{16}$.
> **1.6** $2^4=16$, $2^8=256$, $2^{10}=1024$, $2^{16}=65\,536$. Para 300 valores: $2^8=256<300\le2^9=512$ → **9 bits**.

## 2. Cambios de base

**2.1** Convierte a decimal aplicando el TFN: a) $110110_2$ · b) $10011101_2$ · c) $111111111_2$ · d) $101{,}101_2$ · e) $1100{,}011_2$

**2.2** Convierte a binario: a) $77_{10}$ · b) $200_{10}$ · c) $1023_{10}$ · d) $13{,}625_{10}$ · e) $6{,}1875_{10}$ · f) $0{,}3_{10}$ (en el último, obtén 8 bits fraccionarios).

**2.3** Convierte a binario mediante agrupación de bits: $736_8$ · $1250_8$ · $47{,}6_8$ · $9E_{16}$ · $3B7_{16}$ · $F0{,}A_{16}$

**2.4** Convierte a octal y a hexadecimal: $110101111_2$ · $10110011010_2$ · $11011{,}1_2$

**2.5** Convierte directamente (pasando por binario): a) $574_8$ a hexadecimal · b) $2D9_{16}$ a octal · c) $1777_8$ a hexadecimal

**2.6** Convierte a decimal: $615_{8}$ · $4C2_{16}$ · $ABC_{16}$ · $2304_{5}$ · $1210_{3}$

**2.7** Convierte a octal y a hexadecimal por divisiones sucesivas: $300_{10}$ · $1000_{10}$ · $4095_{10}$

**2.8** ⭐ Convierte $87_{10}$ a base 5 y $211_3$ a base 5.

> [!success]- Solución bloque 2
> **2.1 a)** $1\cdot2^{5} + 1\cdot2^{4} + 0\cdot2^{3} + 1\cdot2^{2} + 1\cdot2^{1} + 0\cdot2^{0} = 54_{10}$
> **2.1 b)** $1\cdot2^{7} + 0\cdot2^{6} + 0\cdot2^{5} + 1\cdot2^{4} + 1\cdot2^{3} + 1\cdot2^{2} + 0\cdot2^{1} + 1\cdot2^{0} = 157_{10}$
> **2.1 c)** $1\cdot2^{8} + 1\cdot2^{7} + 1\cdot2^{6} + 1\cdot2^{5} + 1\cdot2^{4} + 1\cdot2^{3} + 1\cdot2^{2} + 1\cdot2^{1} + 1\cdot2^{0} = 511_{10}$
> **2.1 d)** $1\cdot2^{2} + 0\cdot2^{1} + 1\cdot2^{0} + 1\cdot2^{-1} + 0\cdot2^{-2} + 1\cdot2^{-3} = 5{,}625_{10}$
> **2.1 e)** $1\cdot2^{3} + 1\cdot2^{2} + 0\cdot2^{1} + 0\cdot2^{0} + 0\cdot2^{-1} + 1\cdot2^{-2} + 1\cdot2^{-3} = 12{,}375_{10}$
> **2.2 a)** $1001101_2$
> **2.2 b)** $11001000_2$
> **2.2 c)** $1111111111_2$
> **2.2 d)** $1101{,}101_2$
> **2.2 e)** $110{,}0011_2$
> **2.2 f)** $0{,}01001100_2$ (periódico: $0{,}0100110011\ldots$)
> **2.3** $736_8 = 111\,011\,110_2$ · $1250_8 = 001\,010\,101\,000_2$ · $47{,}6_8 = 100\,111{,}110_2$ · $9E_{16} = 1001\,1110_2$ · $3B7_{16} = 0011\,1011\,0111_2$ · $F0{,}A_{16} = 1111\,0000{,}1010_2$
> **2.4** $110\,101\,111_2 = 657_8$; $1\,1010\,1111_2 = 1AF_{16}$ · $10\,110\,011\,010_2 = 2632_8$; $101\,1001\,1010_2 = 59A_{16}$ · $011\,011{,}100_2 = 33{,}4_8$; $0001\,1011{,}1000_2 = 1B{,}8_{16}$
> **2.5** a) $574_8 = 101\,111\,100_2 = 1\,0111\,1100_2 = 17C_{16}$ · b) $2D9_{16} = 0010\,1101\,1001_2 = 001\,011\,011\,001_2 = 1331_8$ · c) $1777_8 = 001\,111\,111\,111_2 = 0011\,1111\,1111_2 = 3FF_{16}$
> **2.6** $615_{8} = 6\cdot8^{2} + 1\cdot8^{1} + 5\cdot8^{0} = 397_{10}$
> **2.6** $4C2_{16} = 4\cdot16^{2} + 12\cdot16^{1} + 2\cdot16^{0} = 1218_{10}$
> **2.6** $ABC_{16} = 10\cdot16^{2} + 11\cdot16^{1} + 12\cdot16^{0} = 2748_{10}$
> **2.6** $2304_{5} = 2\cdot5^{3} + 3\cdot5^{2} + 0\cdot5^{1} + 4\cdot5^{0} = 329_{10}$
> **2.6** $1210_{3} = 1\cdot3^{3} + 2\cdot3^{2} + 1\cdot3^{1} + 0\cdot3^{0} = 48_{10}$
> **2.7** $300_{10} = 454_8 = 12C_{16}$
> **2.7** $1000_{10} = 1750_8 = 3E8_{16}$
> **2.7** $4095_{10} = 7777_8 = FFF_{16}$
> **2.8** $87_{10} = 322_5$ (87:5 = 17 r 2; 17:5 = 3 r 2) · $211_3 = 2\cdot9+1\cdot3+1 = 22_{10} = 42_5$

## 3. Aritmética binaria (8 bits)

**3.1** Realiza las sumas y comprueba el resultado en decimal: $00101101 + 00100110$ · $01100100 + 00011011$ · $01111111 + 01100011$

**3.2** Realiza las restas en binario directo (con préstamos): $01011010 - 00100011$ · $11001000 - 01110001$ · $01000000 - 00000001$

**3.3** ⭐ Suma $11001000 + 01010000$ con 8 bits. ¿Qué ocurre? ¿Cuántos bits harían falta para que el resultado fuese correcto?

> [!success]- Solución bloque 3
> **3.1** 00101101 + 00100110 = **01010011** (45 + 38 = 83)
> **3.1** 01100100 + 00011011 = **01111111** (100 + 27 = 127)
> **3.1** 01111111 + 01100011 = **11100010** (127 + 99 = 226)
> **3.2** 01011010 − 00100011 = **00110111** (90 − 35 = 55)
> **3.2** 11001000 − 01110001 = **01010111** (200 − 113 = 87)
> **3.2** 01000000 − 00000001 = **00111111** (64 − 1 = 63)
> **3.3** 200 + 80 = 280 > 255: hay **desbordamiento** (*overflow*). Con 8 bits queda 00011000 (= 24) y se pierde el acarreo final. Hacen falta **9 bits**: 100011000.

## 4. Operaciones lógicas

**4.1** Completa la tabla de verdad de NOT A, A AND B, A OR B, A XOR B, A NAND B y A NOR B para todas las combinaciones de A y B.

**4.2** Con $A = 11010110$ y $B = 10011011$, calcula: NOT A, A AND B, A OR B, A XOR B, A NAND B y A NOR B.

**4.3** ¿Qué operación lógica aplicarías con una máscara para: a) poner a 0 los 4 bits de la izquierda de un byte; b) poner a 1 el bit de menor peso; c) invertir todos los bits?

**4.4** ⭐ Con $A=1$, $B=0$, $C=1$, evalúa: a) (A AND B) OR C · b) NOT(A OR B) AND C · c) (A XOR C) NOR B

> [!success]- Solución bloque 4
> **4.1**
>
> | A | B | NOT A | AND | OR | XOR | NAND | NOR |
> | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
> | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 1 |
> | 0 | 1 | 1 | 0 | 1 | 1 | 1 | 0 |
> | 1 | 0 | 0 | 0 | 1 | 1 | 1 | 0 |
> | 1 | 1 | 0 | 1 | 1 | 0 | 0 | 0 |
>
> **4.2** NOT A = 00101001 · AND = 10010010 · OR = 11011111 · XOR = 01001101 · NAND = 01101101 · NOR = 00100000
> **4.3** a) **AND** con 00001111 · b) **OR** con 00000001 · c) **NOT** (o XOR con 11111111).
> **4.4** a) (1·0)+1 = **1** · b) NOT(1)·1 = 0·1 = **0** · c) (1 XOR 1) NOR 0 = 0 NOR 0 = **1**

## 5. Complementos (8 bits)

**5.1** Calcula el complemento a 1 y el complemento a 2 de: $00010110$ · $01100100$ · $10000001$

**5.2** Representa en complemento a 2 con 8 bits: $-5$ · $-37$ · $-100$ · $-128$

**5.3** ¿Qué valor decimal tienen estos números si están en complemento a 2? $11111111$ · $10010110$ · $01111111$

**5.4** Resta en **complemento a 1**: $52-17$ · $30-45$

**5.5** Resta en **complemento a 2**: $100-64$ · $25-73$ · $9-9$

**5.6** ⭐ ¿Cuál es el rango de valores que se pueden representar con 8 bits en: a) binario sin signo; b) signo-magnitud; c) complemento a 1; d) complemento a 2? ¿Por qué el complemento a 2 tiene un valor más?

> [!success]- Solución bloque 5
> **5.1** 00010110: C1 = 11101001, C2 = 11101010
> **5.1** 01100100: C1 = 10011011, C2 = 10011100
> **5.1** 10000001: C1 = 01111110, C2 = 01111111
> **5.2** −5 = 11111011 · −37 = 11011011 · −100 = 10011100 · −128 = 10000000
> **5.3** 11111111 = −1 · 10010110 = −106 · 01111111 = 127
> **5.4** 52 − 17: 00110100 + 11101110 (C1) = 100100010 → se suma el acarreo: **00100011** = 35
> **5.4** 30 − 45: 00011110 + 11010010 (C1) = 11110000 → sin acarreo: resultado negativo en C1, **11110000** = −15 (30 − 45 = −15)
> **5.5** 100 − 64: 01100100 + 11000000 (C2) = 100100100 → se descarta el acarreo: **00100100** = 36
> **5.5** 25 − 73: 00011001 + 10110111 (C2) = 11010000 → sin acarreo: **11010000** = −48
> **5.5** 9 − 9: 00001001 + 11110111 (C2) = 100000000 → se descarta el acarreo: **00000000** = 0
> **5.6** a) 0 a 255 · b) −127 a +127 · c) −127 a +127 · d) **−128 a +127**. En signo-magnitud y en C1 hay dos ceros (+0 y −0); en C2 el cero es único, así que la combinación sobrante (10000000) representa −128.

## 6. Detección de errores

**6.1** Calcula el bit de paridad **par** y el de paridad **impar** de: `1100101` · `0111011` · `1000000`

**6.2** Se reciben estas palabras con paridad **par** (el último bit es el de paridad). ¿Cuáles contienen un error? `10110101` · `01111110` · `11100011`

**6.3** Explica por qué la paridad lineal no detecta un error que cambia **dos** bits a la vez. ¿Puede indicar qué bit ha fallado?

**6.4** Añade paridad bidimensional **par** (columna derecha y fila inferior) a estos datos:
```
1011001
0110110
1110000
```

**6.5** ⭐ Se recibe este bloque con paridad bidimensional par (incluye ya la fila y la columna de paridad). Localiza el bit erróneo y corrígelo:
```
1011 1
0110 0
1100 1
0101 0
```

**6.6** Responde sobre los códigos CRC:
a) ¿Para qué sirven? ¿Corrigen los errores? b) ¿Qué es el polinomio generador G(X) y de qué depende la eficacia del CRC? c) Si G(X) = `11001`, ¿cuántos bits de control se añaden? d) ¿Qué operación se usa en la división? e) ¿Qué indica un resto 0 en el receptor? f) Compara el CRC con la paridad.

**6.7** ⭐ Calcula la trama a transmitir para D(X) = `1101011` con G(X) = `1011`.

**6.8** ⭐ Con G(X) = `1101`, comprueba si las tramas recibidas `1011100` y `1011111` son correctas.

> [!success]- Solución bloque 6
> **6.1** `1100101` tiene 4 unos → par = **0**, impar = **1**
> **6.1** `0111011` tiene 5 unos → par = **1**, impar = **0**
> **6.1** `1000000` tiene 1 uno → par = **1**, impar = **0**
> **6.2** `10110101`: 5 unos → **error**
> **6.2** `01111110`: 6 unos → correcta (no se detecta error)
> **6.2** `11100011`: 5 unos → **error**
> **6.3** Al cambiar dos bits, el número de unos sigue siendo par (o impar), así que la paridad no varía. La paridad lineal solo detecta un número **impar** de errores y **no** indica qué bit ha fallado.
> **6.4**
> ```
> 1011001 0
> 0110110 0
> 1110000 1
> 0011111 1   <- fila de paridad
> ```
> **6.5** La fila 3 y la columna 2 tienen paridad impar → el bit erróneo está en su cruce. La fila corregida es `1000`.
> **6.6** a) Detectan errores de almacenamiento o transmisión añadiendo bits de control (redundancia); **no los corrigen**. b) Es el divisor común que usan emisor y receptor; la eficacia depende de él (CRC-12, CRC-16, CRC-32…). c) Grado 4 → **4 bits**. d) División módulo 2: **XOR**, sin acarreos. e) Que no se ha detectado error. f) El CRC detecta más errores, pero es más redundante.
> **6.7** Se añaden 3 ceros y se divide entre 1011:
> ```
> 1101011000 / 1011
> 1101
> 1011
> ----
>  1100
>  1011
>  ----
>   1111
>   1011
>   ----
>    1001
>    1011
>    ----
>     0100
>     0000
>     ----
>      1000
>      1011
>      ----
>       0110
>       0000
>       ----
>        110  <- resto
> ```
> Resto **110** → trama: **1101011110**
> **6.8** `1011100` → resto 000 → **correcta**
> **6.8** `1011111` → resto 011 → **error detectado**

## 7. Codificación de la información

**7.1** Clasifica como código de longitud fija o variable: ASCII, código Morse, BCD, UTF-8.

**7.2** Ordena de menor a mayor: 1 GB, 1 GiB, 1000 MiB, 1 TB, 1 TiB.

**7.3** Calcula: a) bytes de 3 KiB y de 3 kB · b) MiB de 52 428 800 B · c) ⭐ ¿Cuántos GiB muestra el sistema operativo para un disco anunciado como 2 TB?

**7.4** Codifica en BCD: 2026 · 705 · 39. Decodifica: `1001 0000 0111` y `0011 0110 0100 0001`.

**7.5** Codifica en decimal **empaquetado** y **desempaquetado**: +815 · −62 · −1009 (signo + = 1100, − = 1101).

**7.6** Representa en **signo-magnitud con 8 bits**: $+45$, $-45$, $-127$. ¿Cuántas representaciones tiene el cero?

**7.7** ⭐ Representa en IEEE 754 de **simple precisión** (indica signo, exponente, mantisa y resultado en hexadecimal): a) $5{,}5$ · b) $-0{,}375$ · c) $100{,}25$

**7.8** ⭐ ¿Qué número decimal representa el dato IEEE 754 de simple precisión `C1 48 00 00`₁₆? ¿Y `3F A0 00 00`₁₆?

> [!success]- Solución bloque 7
> **7.1** Longitud fija: ASCII y BCD · longitud variable: Morse y UTF-8 (1 a 4 bytes por carácter).
> **7.2** **1 GB < 1000 MiB < 1 GiB < 1 TB < 1 TiB** (1 GB = 10⁹; 1000 MiB ≈ 1,0486·10⁹; 1 GiB ≈ 1,0737·10⁹; 1 TB = 10¹²; 1 TiB ≈ 1,0995·10¹²).
> **7.3** a) 3 KiB = 3072 B; 3 kB = 3000 B · b) 52 428 800 / 2²⁰ = **50 MiB** · c) 2·10¹² / 2³⁰ ≈ **1862,65 GiB** (que el sistema suele rotular como «GB»).
> **7.4** 2026 = 0010 0000 0010 0110 · 705 = 0111 0000 0101 · 39 = 0011 1001 · `1001 0000 0111` = **907** · `0011 0110 0100 0001` = **3641**
> **7.5** +815: empaquetado **1000 0001 0101 1100** · desempaquetado **1111 1000  1111 0001  1100 0101**
> **7.5** −62: empaquetado **0110 0010 1101** · desempaquetado **1111 0110  1101 0010**
> **7.5** −1009: empaquetado **0001 0000 0000 1001 1101** · desempaquetado **1111 0001  1111 0000  1111 0000  1101 1001**
> **7.6** +45 = **0**0101101 · −45 = **1**0101101 · −127 = **1**1111111. El cero tiene **dos** representaciones: 00000000 (+0) y 10000000 (−0).
> **7.7** 5,5: $5{,}5 = 101{,}1_2 = 1{,}011\cdot2^2$ → signo 0, exponente 2 + 127 = 129 = 10000001, mantisa 01100000000000000000000 → **40B00000₁₆**
> **7.7** −0,375: $0{,}375 = 0{,}011_2 = 1{,}1\cdot2^{-2}$ → signo 1, exponente −2 + 127 = 125 = 01111101, mantisa 10000000000000000000000 → **BEC00000₁₆**
> **7.7** 100,25: $100{,}25 = 1100100{,}01_2 = 1{,}10010001\cdot2^6$ → signo 0, exponente 6 + 127 = 133 = 10000101, mantisa 10010001000000000000000 → **42C88000₁₆**
> **7.8** C1480000 = 1 10000010 10010000000000000000000 → signo −, exponente 130−127 = 3, mantisa 1,1001 → **−12,5**
> **7.8** 3FA00000 = 0 01111111 01000000000000000000000 → signo +, exponente 127−127 = 0, mantisa 1,01 → **1,25**

## 8. Repaso tipo test

Indica si cada afirmación es **verdadera** o **falsa**:

1. Un nibble tiene 4 bits y un byte 8 bits.
2. Según el SI, 1 kB = 1024 B.
3. Cada dígito octal equivale a 4 bits.
4. El complemento a 2 de un número es su complemento a 1 más 1.
5. En la resta en complemento a 1, el acarreo final se descarta.
6. La paridad bidimensional permite localizar un error simple.
7. El CRC corrige automáticamente los errores detectados.
8. En IEEE 754 de simple precisión el exponente tiene 8 bits y un sesgo de 127.
9. En el decimal empaquetado cada dígito ocupa un byte.
10. UTF-8 es compatible con ASCII.

> [!success]- Solución bloque 8
> 1 V · 2 F (1000 B; 1024 B es 1 KiB) · 3 F (3 bits) · 4 V · 5 F (en C1 se **suma** al resultado; se descarta en C2) · 6 V · 7 F (solo detecta) · 8 V · 9 F (un nibble; un byte es el desempaquetado) · 10 V

🡠 [[Sistemas Informáticos/Unidad 0 Sistemas de numeración/Apuntes/2. Codificación de la información\|2. Codificación de la información]] ⮅ [[Sistemas Informáticos/Unidad 0 Sistemas de numeración/Prácticas/T2 - Ud 0 Relación de ejercicios preparatorios#📝 Relación de ejercicios preparatorios · Unidad 0\|#📝 Relación de ejercicios preparatorios · Unidad 0]] 🡢 [[Sistemas Informáticos/Unidad 0 Sistemas de numeración/Prácticas/T1 - Ud 0 Cambios_de_base\|T1 - Ud 0 Cambios_de_base]]

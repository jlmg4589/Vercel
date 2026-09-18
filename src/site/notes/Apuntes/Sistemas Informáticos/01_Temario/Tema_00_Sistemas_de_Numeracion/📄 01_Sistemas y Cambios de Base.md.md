---
{"dg-publish":true,"permalink":"/apuntes/sistemas-informaticos/01-temario/tema-00-sistemas-de-numeracion/01-sistemas-y-cambios-de-base-md/","dg-note-properties":{}}
---

# 01. Sistemas y Cambios de Base

- **Asignatura:** Sistemas Informáticos
- **Tema:** Tema 0 - Sistemas de Numeración y Codificación

---

## 1. Representación de la Información

La función principal del **ordenador** es automatizar la información. Tanto para almacenar la información que le suministremos, como para procesarla o transmitirla a otro ordenador, es necesario que la transforme a un sistema que pueda entender.

Cualquier dato que nosotros le suministremos al ordenador, internamente será transformado a un conjunto de 0 y 1 (dígitos binarios o bits). De forma contraria, cuando necesitamos un dato almacenado, este transformará el conjunto de 0 y 1 en una entidad legible por el usuario.

### 1.1 Tipos de datos atendiendo al lugar que ocupen dentro del proceso
- **Datos de entrada:** Hacen referencia a los datos que introduce el usuario al ordenador (teclado, micrófono o cualquier periférico de entrada).
- **Datos internos del proceso:** Son los datos que utiliza el ordenador internamente para realizar operaciones antes de mostrar los datos.
- **Datos de salida:** Son directamente los resultados del proceso de automatizar la información.

![Procesado de datos.png](/img/user/Apuntes/Sistemas%20Inform%C3%A1ticos/03_Recursos/Imagenes/Procesado%20de%20datos.png)

### 1.2. Sistemas de numeración
Podríamos definirlo, como un conjunto de símbolos y reglas que se utilizan para representar cantidades o valores numéricos _p. ej. {0,1,2,3,4,5,6,7,8,9}_.

Los sistemas de numeración pueden clasificarse en _posicionales_ o _no posicionales_.

- <u>*Posicionales*</u>: el valor de cada símbolo se determina por su valor y por la posición que ocupe, p. ej. el sistema decimal.

Este tipo de sistemas de numeración se caracterizan por la **base**, que es un valor que indica el número máximo de símbolos diferentes que podemos usar en ese sistema para representar cada cantidad numérica.

- <u>*No posicionales*</u>: cada símbolo tiene un valor independientemente de la posición que ocupe, p. ej. los números romanos.

Con el **teorema fundamental de la numeración**, podemos relacionar una cantidad expresada en cualquier sistema de numeración con la misma cantidad expresada en el sistema decimal.

$$N = \sum_{i=-j}^{k-1} X_i \cdot B^i$$

- **N**: valor de la cantidad que queremos expresar en el sistema de numeración decimal.

- **k**: número de dígitos de la parte entera del número (a la izquierda de la coma).

- **j**: número de dígitos de la parte fraccionariadel número (a la derecha de la coma).

- **B**: base del sistema de numeración en que está expresado el número.

- **X**: son cada los dígitos del número (el dígito a la izquierda de la coma ocupa el lugar 0).


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/apuntes/sistemas-informaticos/02-practicas/00-actividades-del-temario-md/#u-actividad-1-u" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



## <u>Actividad 1</u>

Utiliza el **Teorema fundamental de la numeración** para ver cómo se calcularía el valor de cada dígito de los siguientes números en decimal aunque la base original es ya de por sé decimal:

1) 1785->
2) 345,65->
3) 12,357->


---



</div></div>


---


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/apuntes/sistemas-informaticos/02-practicas/00-actividades-del-temario-md/#u-sol-actividad-1-u" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



## <u>Sol_Actividad 1</u>

1) $1785 -> 5\cdot{10}^{0}+8\cdot{10}^{1}+7\cdot{10}^{2}+1\cdot{10}^{3}$
2) $345,65 -> 5\cdot{10}^{-2}+6\cdot{10}^{-1}+5\cdot{10}^{0}+4\cdot{10}^{1}+3\cdot{10}^{2}$
3) $12,357 -> 7\cdot{10}^{-3}+5\cdot{10}^{-2}+3\cdot{10}^{-1}+2\cdot{10}^{0}+1\cdot{10}^{1}$

</div></div>


---
Esto nos va a servir como base de aprendizaje para los distintos sistemas de numeración que vamos a conocer, como *ocal*, *hexadecimal* o el *binario*.

### Binario

También llamado en **_base 2_**. Utiliza dos símbolos diferentes: el cero y el uno **_{0,1}_**.

El *sistema binario* es el que utiliza internamente el ordenador, realmente el ordenador no trabaja con 0 o 1, sino que utiliza para ello señales eléctricas. Un voltaje positivo podría significar un 1 y un valor nulo un 0.

<div style="display: flex; gap: 10px;">

![Señal.png\|320](/img/user/Apuntes/Sistemas%20Inform%C3%A1ticos/03_Recursos/Imagenes/Se%C3%B1al.png) ![Transistores.png\|320](/img/user/Apuntes/Sistemas%20Inform%C3%A1ticos/03_Recursos/Imagenes/Transistores.png)

</div>

### Octal

Conocido también como sistema ***en base 8***, utiliza los símbolos ***{0,1,2,3,4,5,6,7}***. Se caracteriza por tener una correlación directa con el sistema binario. Cada símbolo en *octal* se representa con **3 símbolos en binario**.
En la siguiente tabla podemos ver la correspondencia entre *decimal*, *octal* y *binario*.

| Decimal | Octal | Binario |
| :-: | :-: | :-: |
| 0 | 0 | 000 |
| 1 | 1 | 001 |
| 2 | 2 | 010 |
| 3 | 3 | 011 |
| 4 | 4 | 100 |
| 5 | 5 | 101 |
| 6 | 6 | 110 |
| 7 | 7 | 111 |


### Hexadecimal

Conocido también como sistema en ***base 16***, al utilizar 16 símbolos diferentes, se utilizan los números del 0 al 9 y para completarlos se utilizan las primeras 6 letras del abecedario,
por tanto el conjunto sería ***{0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F}***.
Este sistema de numeración también tiene una correlación
directa con el sistema binario. Cada símbolo en hexadecimal se representa con 4 símbolos en binario.
En la tabla de la derecha podemos ver la correspondencia
entre *decimal*, *hexadecimal* y *binario*.

| Decimal | Hexadecimal | Binario |
| :-: | :-: | :-: |
| 0 | 0 | 0000 |
| 1 | 1 | 0001 |
| 2 | 2 | 0010 |
| 3 | 3 | 0011 |
| 4 | 4 | 0100 |
| 5 | 5 | 0101 |
| 6 | 6 | 0110 |
| 7 | 7 | 0111 |
| 8 | 8 | 1000 |
| 9 | 9 | 1001 |
| 10 | A | 1010 |
| 11 | B | 1011 |
| 12 | C | 1100 |
| 13 | D | 1101 |
| 14 | E | 1110 |
| 15 | F | 1111 |
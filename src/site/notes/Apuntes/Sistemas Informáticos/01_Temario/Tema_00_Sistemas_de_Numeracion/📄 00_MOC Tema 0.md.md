---
{"dg-publish":true,"permalink":"/apuntes/sistemas-informaticos/01-temario/tema-00-sistemas-de-numeracion/00-moc-tema-0-md/","tags":["gardenEntry"],"dg-note-properties":{}}
---


# Tema 0: Sistemas de Numeración y Codificación

- **Estado:** #estado/pendiente
- **Asignatura:** Sistemas Informáticos

---

## 📌 Índice de Contenidos

### 1. Representación y Bases
- [[01_Sistemas y Cambios de Base\|Sistemas Posicionales y Teorema Fundamental]]
- **Sistemas principales:** Binario, Octal, Hexadecimal
- **Conversiones:**
  - Binario ↔ Decimal (TFN y divisiones sucesivas)
  - Binario ↔ Octal (grupos de 3 bits)
  - Binario ↔ Hexadecimal (grupos de 4 bits)

### 2. Operaciones en Binario
- [[02_Aritmetica y Algebra de Boole\|Operaciones Aritméticas y Lógicas]]
  - Suma y resta binaria (acarreo)
  - Puertas lógicas: `NOT`, `AND`, `OR`, `XOR`, `NAND`, `NOR`

### 3. Representación Negativa y Complementos
- [[03_Complementos y Resta\|Complemento a 1 y Complemento a 2]]
  - Cálculo de $C_1$ (NOT)
  - Cálculo de $C_2$ ($C_1 + 1$)
  - Restas mediante $C_1$ y $C_2$

### 4. Codificación Numérica y Almacenamiento
- [[04_Representacion Numérica Avanzada\|Formatos de Almacenamiento]]
  - Unidades de medida (SI vs. Unidades binarias / IEC)
  - Enteros: BCD, Decimal empaquetado/desempaquetado, Binario puro con signo
  - Coma flotante: Estándar IEEE 754 (Simple y doble precisión)

### 5. Control de Calidad de Datos
- [[05_Deteccion de Errores\|Métodos de Detección de Errores]]
  - Paridad lineal (par / impar)
  - Paridad bidimensional
  - Códigos de Redundancia Cíclica (CRC)

---

## 🎯 Fórmulas Clave
- **Teorema Fundamental de la Numeración:**
  $$N = \sum_{i=-j}^{k-1} X_i \cdot B^i$$
- **Complemento a 2:** $C_2 = C_1 + 1$
- **Exponente Coma Flotante (IEEE 754):** $E = e + (2^{n-1} - 1)$ (Bias = 127 para Simple Precisión)
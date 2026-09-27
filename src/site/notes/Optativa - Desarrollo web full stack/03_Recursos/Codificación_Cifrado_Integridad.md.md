---
{"dg-publish":true,"permalink":"/optativa-desarrollo-web-full-stack/03-recursos/codificacion-cifrado-integridad-md/","noteIcon":"","created":"2026-09-27T13:23:21.669+02:00","updated":"2026-09-27T13:23:21.668+02:00","dg-note-properties":{}}
---


<div style="text-align: center;">
<h1>Codificación, Cifrado e integridad de la información</h1>
</div>

## **Alcance y propósito**

Este documento reúne conceptos operativos y prácticos para la protección de la información en entornos de bastionado y redes: codificación, cifrado (simétrico y asimétrico), integridad, herramientas y buenas prácticas. El objetivo es ofrecer una referencia clara y accesible, sin entradas de propuesta o contenido interactivo.

## **Índice**

- [Alcance y propósito](#alcance-y-propósito)

- [Índice](#índice)

- [Terminología y alcance](#terminología-y-alcance)

- [Codificación](#codificación)

- [Codificación con UTF](#codificación-con-utf)

- [Codificación Base64 y Hexadecimal](#codificación-base64-y-hexadecimal)

- [Normalización Unicode: NFC vs NFD](#normalización-unicode-nfc-vs-nfd)

- [Cifrado](#cifrado)

- [Cifrado simétrico](#cifrado-simétrico)

- [Cifrado asimétrico](#cifrado-asimétrico)

- [Integridad](#integridad)

- [Checksum, CRC y funciones hash](#checksum-crc-y-funciones-hash)

- [Autenticación de la integridad](#autenticación-de-la-integridad)

- [Anexo I](#anexo-i)

## **Terminología y alcance**

- **Codificación:** transformación para representación/trasmisión (ej. UTF-8, Base64). No aporta confidencialidad.

- **Cifrado:** transformación con claves que protege confidencialidad e integridad (cuando corresponde).

- **Integridad:** mecanismos para detectar/modificar datos (checksums, CRC, hashes, HMAC, firmas).

El documento prioriza prácticas aplicables a servidores y comunicaciones de red, así como la interoperatividad con herramientas comunes (OpenSSL, libsodium, herramientas UNIX).
## **Codificación**

Antes de proteger o asegurar la información, necesitas entender cómo se representa esa información en un sistema.

La codificación transforma datos para su representación o transporte y cualquiera que conozca la regla (el estándar de codificación) puede decodificarla y leerla. Con esto se indica que la codificación no aporta confidencialidad.
### **Codificación con UTF**

Use `UTF-8` por defecto para texto y declare la codificación en APIs y archivos. `UTF-8` es el estándar de codificación de Unicode más utilizado en la web y ofrece un buen equilibrio entre compatibilidad (con ASCII) y soporte para casi todos los caracteres del mundo. También se conocen sus variantes `UTF-16` y `UTF-32`.

**Tabla para codificación en castellano**

| Codificación | Puntos de Código Utilizados | Rango Hexadecimal | Unidades de Código/Bytes |
| :--- | :--- | :--- | :--- |
| **UTF-8** (Longitud variable: 1 o 2) | Caracteres ASCII (Letras básicas, números, puntuación) | $\text{U+0000}$ a $\text{U+007F}$ | **1 byte** |
| | Caracteres extendidos (ñ, tildes, ¿, ¡) | $\text{U+0080}$ a $\text{U+00FF}$ | **2 bytes** |
| **UTF-16** (Longitud fija) | Todos los caracteres comunes del castellano | $\text{U+0000}$ a $\text{U+00FF}$ | **2 bytes** (1 unidad de 16 bits) |
| **UTF-32** (Longitud fija) | Todos los caracteres del castellano | $\text{U+0000}$ a $\text{U+00FF}$ | **4 bytes** (1 unidad de 32 bits) |

### **Codificación Base64 y Hexadecimal**

El empaquetamiento de información en `Base64` se utiliza fundamentalmente cuando se necesita transmitir datos binarios (que pueden ser cualquier cosa, desde una imagen hasta texto cifrado) a través de canales o protocolos diseñados únicamente para manejar texto (más compacto que `hex`).

Casos principales donde la información debe ser codificada en `Base64`:

* Protocolos de Comunicación Basados en Texto.
* Inclusión de Datos Binarios en Formatos Textuales.
* Contextos Criptográficos y de Seguridad.

**Codificar con Base64:**

```bash
# Codifica el contenido de un archivo (o STDIN) a Base64
echo -n "Mensaje secreto" | base64
	# Salida: TWVuc2FqZSBzZWNyZXRv

# Codificar un archivo
base64 archivo.bin > archivo.b64
```

**Descodificar con Base64:**

```bash
# Decodifica una cadena Base64 de vuelta a su forma original (binaria/texto)
echo -n "TWVuc2FqZSBzZWNyZXRv" | base64 -d
	# Salida: Mensaje secreto

# Decodificar un archivo
base64 -d archivo.b64 > archivo.bin
```

**Codificar a Hexadecimal:**

```bash
# Crea un volcado hexadecimal del mensaje (dos caracteres hex por byte)
echo -n "Sí" | xxd -p
	# Salida (para UTF-8 NFC): 53c3ad
```

**Decodificar de Hexadecimal:**

```bash
# Convierte una cadena hexadecimal de vuelta a bytes
echo -n "53c3ad" | xxd -p -r
	# Salida: Sí
```

<!--Comentarios-->

> **Nota:** El argumento **-n** es crucial para evitar el salto de línea al final.\

No confundir codificar con cifrar; Base64/hex no otorgan confidencialidad.
### **Normalización Unicode: NFC vs NFD**

NFC (Normalization Form C) y NFD (Normalization Form D) son estándares de Unicode que definen cómo se representan los caracteres "especiales" (como letras con tildes, diéresis o símbolos) a nivel de bytes (bits).

El problema que resuelven es que existen varias formas de escribir la misma letra para una computadora, aunque para el ojo humano se vean idénticas.

Forma de Normalización por Composición Canónica (NFC) antes de comparar, "*hashear*" o firmar texto. Es un proceso definido por el estándar Unicode que transforma una cadena de texto para que todos los caracteres equivalentes canónicamente tengan una representación binaria única (una forma normalizada).

**Ejemplo: Flujo de Datos para "Sí" (Comenzando en NFD)**

Este ejercicio ilustra la importancia de la normalización NFC para prevenir la divergencia de bytes, que es un riesgo de seguridad e integridad.

1. UNICODE (La Forma Descompuesta - NFD)

Asumimos que la palabra fue ingresada en una forma que generó la Normalización Descompuesta (NFD). El carácter 'í' se almacena como dos puntos de código separados.

| Carácter | Punto de Código (Unicode) | Descripción |
| :--- | :--- | :--- |
| S | `U+0053` | Letra base ASCII. |
| i | `U+0069` | Letra base 'i'. |
| ́ | `U+0301` | Acento agudo combinatorio (diacrítico). |
| **SECUENCIA NFD:** | **`U+0053 U+0069 U+0301`** | Tres puntos de código. |

2. UTF-8 (Codificación de la Secuencia NFD)

El codificador UTF-8 traduce los tres puntos de código de la secuencia NFD a una secuencia de bytes para almacenamiento.

| Punto de Código | Representación en Bytes (Hexadecimal) |
| :--- | :--- |
| `U+0053` ('S') | `53` (1 byte) |
| `U+0069` ('i') | `69` (1 byte) |
| `U+0301` ('́') | `CC 81` (2 bytes, ya que es $\text{U} > \text{U+007F}$) |
| **SECUENCIA DE BYTES NFD:** | **`53 69 CC 81`** |

La Divergencia Crítica de Bytes

| Forma de Normalización | Secuencia de Bytes UTF-8 | Longitud |
| :--- | :--- | :--- |
| **NFD (Recibido)** | `53 69 CC 81` | 4 bytes |
| **NFC (Esperado)** | `53 C3 AD` | 3 bytes |

* **Error**: Una comparación binaria o de hash simple (NFD vs. NFC) fallará porque las secuencias de bytes son completamente diferentes (69 CC 81 != C3 AD).

* **Solución**: La Normalización NFC debe ser aplicada explícitamente para convertir la secuencia 53 69 CC 81 a 53 C3 AD antes de realizar cualquier tratamiento, por ejemplo calcular el hash.

```bash
# Si queremos comprobar un hash o integridad, debemos normalizar primero a NFC

printf '\x53\x69\xcc\x81' | uconv -x nfc | openssl passwd -salt 555 -5 -stdin

# Con este ejemplo podemos comprobar cómo forzar que la interpretación sea NFC antes de calcular una función hash o similar.
```

>**Conclusión**: La Codificación (`UTF-8`) solo convierte los puntos de código que se le dan; no realiza la normalización. Por lo tanto, si la entrada está en NFD, el resultado en bytes y el hash serán diferentes a la forma NFC esperada, creando un error de integridad.

## **Cifrado**

El Cifrado es el concepto más complejo, ya que implica la gestión de claves y la protección de la confidencialidad. De esta forma, el cifrado nos protege desde el envío de un correo personal hasta conrolar el acceso a nuestros archivos de forma que sólo el personal autorizado pueda acceder a ellos.

### **Cifrado simétrico**

![Pasted image 20260922111901.png](/img/user/Optativa%20-%20Desarrollo%20web%20full%20stack/03_Recursos/Imagenes/Pasted%20image%2020260922111901.png)
  
- Misma clave para cifrar/descifrar.
- Adecuado para datos grandes y rendimiento por su rapidez (relativo a asimétrico).
- Use algoritmos AEAD: `AES-GCM`, `ChaCha20-Poly1305`.
- No reutilice nonces/IV con la misma clave.
- Derive claves de contraseñas con KDFs (Argon2, scrypt, PBKDF2 si es necesario).

1. Ejemplo de cifrar y descifrar con la herramienta `gpg`

```bash
# Creamos un archivo cualquiera.
echo "Esto es un secreto" > archivo.txt

# Ciframos con valores por defecto
gpg -c archivo.txt

# Si queremos total seguridad debemos eliminar el archivo original.
rm archivo.txt

# En el caso de que no nos pida la contraseña al descifrar, usamos el comando que reinicia el agente y borra las claves en memoria
gpgconf --reload gpg-agent

# Para descifrar
gpg archivo.txt.gpg
```

2. Ejemplo de cifrar y descifrar con la herramienta `openssl`

```bash
# Creamos un archivo cualquiera.
echo "Esto es un secreto" > archivo.txt

# Ciframos con AES-256-CBC
openssl enc -aes-256-cbc -pbkdf2 -in archivo.txt -out archivo.enc

# Si queremos total seguridad debemos eliminar el archivo original.
rm archivo.txt

# Para descifrar
openssl enc -d -aes-256-cbc -pbkdf2 -in archivo.enc -out archivo_recuperado.txt
```

  3. Ejemplo de cifrar y descifrar con la herramienta openssl pero usando un archivo como clave y encriptando éste.

```bash
# Creamos un archivo cualquiera.
echo "Esto es un secreto" > archivo.txt

# Generamos una clave aleatoria de 4KB y la guardamos en llave_maestra.bin
dd if=/dev/urandom of=llave_maestra.bin bs=1024 count=4

# Ciframos la clave maestra con una clave mental.
openssl enc -aes-256-cbc -pbkdf2 -in llave_maestra.bin -out llave_maestra.enc

# Eliminamos el archivo llave_maestra.bin por seguridad.
rm llave_maestra.bin

# Ahora ciframos los datos de "archivo.txt" usando la clave maestra y la contraseña mental.
openssl enc -d -aes-256-cbc -pbkdf2 -in llave_maestra.enc | \

openssl enc -aes-256-cbc -pbkdf2 -in archivo.txt -out archivo.enc -pass stdin

# Eliminamos el archivo original.
rm archivo.txt

# --------- DESCIFRANDO ---------
openssl enc -d -aes-256-cbc -pbkdf2 -in llave_maestra.enc | \

openssl enc -d -aes-256-cbc -pbkdf2 -in archivo.enc -out secretos_recuperados.txt -pass stdin
```

  ### **Cifrado asimétrico**


![Pasted image 20260922112303.png](/img/user/Optativa%20-%20Desarrollo%20web%20full%20stack/03_Recursos/Imagenes/Pasted%20image%2020260922112303.png)
  
En lugar de una llave, ahora tienes un par de llaves matemáticamente vinculadas, pero diferentes:

- Par de claves pública/privada. Usado para intercambio de claves, cifrado puntual y firmas.
- Preferir curvas modernas para ECC (Curve25519/Ed25519) o RSA con longitudes adecuadas (≥3072 para mayor margen).
- La **llave Privada** (La llave maestra): **NUNCA** sale de tu posesión. Sirve para abrir lo que te enviaron cifrado o para firmar documentos.

  1. Ejemplo de confidencialidad usando GPG:

```bash
# --------- LADO DEL EMISOR ----------

# Generar par de claves
gpg --full-generate-key

# Exportar clave pública para compartir y se la enviamos por cualquier medio. No importa que nos la intercepten, lo importante es que la clave privada nunca salga de nuestro poder.

gpg --output mi_publica.gpg --export tu_email@ejemplo.com

  

# ---------- LADO DEL RECEPTOR ----------

# Creamos el archivo a cifrar.
echo "Este es un mensaje secreto para ti" > archivo_secreto.txt

# Importar clave pública de otra persona
gpg --import mi_publica.gpg

# Cifrar un archivo para el emisor de la clave pública.
gpg --encrypt --recipient tu_email@ejemplo.com archivo_secreto.txt

# Eliminamos el archivo original por seguridad.
rm archivo_secreto.txt

# Por último se lo enviamos al emisor de la clave pública.

# --------- LADO DEL EMISOR ----------

# La otra persona descifra con su clave privada
gpg --decrypt --recipient tu_email@ejemplo.com archivo_secreto.txt.gpg
```

  2. Ejemplo de autenticación con SSH sin contraseña:

```bash
# Generar par de claves ED25519
ssh-keygen -t ed25519 -C "tu_email@ejemplo.com"

# La clave privada queda en ~/.ssh/id_ed25519 y la pública en ~/.ssh/id_ed25519.pub

# Copiar la clave pública al servidor remoto dentro del archivo ~/.ssh/authorized_keys
ssh-copy-id -i ~/.ssh/id_ed25519.pub usuario@servidor_remoto

# Ahora podemos conectarnos sin contraseña
ssh usuario@servidor_remoto
```

3. Integridad y No repudio (Firma Digital):

```bash
# Crea un documento cualquiera.
echo "Este es un documento importante" > documento.txt

# Firmar el documento con nuestra clave privada

# --clearsign crea un archivo de texto legible con la firma al final
gpg --clearsign documento.txt

# Verificar la firma con la clave pública del emisor
gpg --verify documento.txt.asc
```

  
> **Nota: **
>- Si queremos ver la lista de claves importadas en nuestro llavero:
>```bash
>gpg --list-keys
>```
>- Si queremos usar una clave concreta para firmar:
>```bash
>gpg -u tu_email@ejemplo.com --clearsign documento.txt
>```
## **Integridad**

La integridad verifica que los datos no han sido alterados, y es el último eslabón en la cadena de protección de la información. Los mecanismos de integridad pueden detectar modificaciones accidentales o maliciosas.

  ### **Checksum, CRC y funciones hash**

- **Checksum:** detección básica de errores, no resistente a adversarios.
- **CRC:** robusto para errores de transmisión, no adecuado como mecanismo de seguridad frente a ataques dirigidos.
- **Funciones hash criptográficas:** SHA-256, SHA-3 para huellas; use funciones actuales (evitar MD5/SHA-1 para seguridad).
### **Autenticación de la integridad**

- **HMAC:** hash con clave (ej. HMAC-SHA256) para autenticar mensajes con una clave compartida.
- **AEAD:** cifrado autenticado que protege confidencialidad e integridad en un único paso.
- **Firmas digitales:** para integridad con no-repudio (RSA-PSS, ECDSA, Ed25519).

1. Ejemplo de checksum simple (no seguro):

```bash
# Crear un archivo
echo "Datos importantes" > archivo.txt

# Calcular checksum MD5
md5sum archivo.txt

# Modificar el archivo
echo "Modificación maliciosa" >> archivo.txt

# Recalcular checksum MD5
md5sum archivo.txt
```

2. Ejemplo de CRC32 (no seguro):

```bash
# Crear un archivo
echo "Datos importantes" > archivo.txt

# Calcular CRC32
cksum archivo.txt

# Modificar el archivo
echo "Modificación maliciosa" >> archivo.txt

# Recalcular CRC32
cksum archivo.txt
```

3. Ejemplo de función hash segura:

```bash
# Crear un archivo
echo "Datos importantes" > archivo.txt

# Calcular SHA-256
sha256sum archivo.txt

# Modificar el archivo
echo "Modificación maliciosa" >> archivo.txt

# Recalcular SHA-256
sha256sum archivo.txt
```

4. Ejemplo de HMAC:

```bash
# Crear un archivo
echo "Datos importantes" > archivo.txt

# Calcular HMAC-SHA256 con clave "
openssl dgst -sha256 -hmac "mi_clave_secreta" archivo.txt

# Modificar el archivo
echo "Modificación maliciosa" >> archivo.txt

# Recalcular HMAC-SHA256
openssl dgst -sha256 -hmac "mi_clave_secreta" archivo.txt
```

# Anexo I

1. **Bloque ASCII Estándar (0 - 127)**

Es la base de la informática. Los primeros 32 son comandos de control (invisibles) y el resto son caracteres imprimibles.
 

2. **Caracteres Imprimibles (32-126)**

|  Dec   | Hex |  Char  | Descripción       |  Dec   | Hex |  Char  | Descripción     |   Dec   | Hex |  Char  | Descripción  |
| :----: | :-: | :----: | :---------------- | :----: | :-: | :----: | :-------------- | :-----: | :-: | :----: | :----------- |
| **32** | 20  |        | Espacio           | **64** | 40  | **@**  | Arroba          | **96**  | 60  | **`**  | Acento grave |
| **33** | 21  | **!**  | Exclamación       | **65** | 41  | **A**  | A Mayúscula     | **97**  | 61  | **a**  | a minúscula  |
| **34** | 22  | **"**  | Comillas dobles   | **66** | 42  | **B**  | B Mayúscula     | **98**  | 62  | **b**  | b minúscula  |
| **35** | 23  | **#**  | Almohadilla       | **67** | 43  | **C**  | C Mayúscula     | **99**  | 63  | **c**  | c minúscula  |
| **36** | 24  | **$**  | Dólar             | **68** | 44  | **D**  | D Mayúscula     | **100** | 64  | **d**  | d minúscula  |
| **37** | 25  | **%**  | Porcentaje        | **69** | 45  | **E**  | E Mayúscula     | **101** | 65  | **e**  | e minúscula  |
| **38** | 26  | **&**  | Ampersand         | **70** | 46  | **F**  | F Mayúscula     | **102** | 66  | **f**  | f minúscula  |
| **39** | 27  | **'**  | Comilla simple    | **71** | 47  | **G**  | G Mayúscula     | **103** | 67  | **g**  | g minúscula  |
| **40** | 28  | **(**  | Paréntesis abre   | **72** | 48  | **H**  | H Mayúscula     | **104** | 68  | **h**  | h minúscula  |
| **41** | 29  | **)**  | Paréntesis cierra | **73** | 49  | **I**  | I Mayúscula     | **105** | 69  | **i**  | i minúscula  |
| **42** | 2A  | **\*** | Asterisco         | **74** | 4A  | **J**  | J Mayúscula     | **106** | 6A  | **j**  | j minúscula  |
| **43** | 2B  | **+**  | Más               | **75** | 4B  | **K**  | K Mayúscula     | **107** | 6B  | **k**  | k minúscula  |
| **44** | 2C  | **,**  | Coma              | **76** | 4C  | **L**  | L Mayúscula     | **108** | 6C  | **l**  | l minúscula  |
| **45** | 2D  | **-**  | Guion             | **77** | 4D  | **M**  | M Mayúscula     | **109** | 6D  | **m**  | m minúscula  |
| **46** | 2E  | **.**  | Punto             | **78** | 4E  | **N**  | N Mayúscula     | **110** | 6E  | **n**  | n minúscula  |
| **47** | 2F  | **/**  | Barra             | **79** | 4F  | **O**  | O Mayúscula     | **111** | 6F  | **o**  | o minúscula  |
| **48** | 30  | **0**  | Cero              | **80** | 50  | **P**  | P Mayúscula     | **112** | 70  | **p**  | p minúscula  |
| **49** | 31  | **1**  | Uno               | **81** | 51  | **Q**  | Q Mayúscula     | **113** | 71  | **q**  | q minúscula  |
| **50** | 32  | **2**  | Dos               | **82** | 52  | **R**  | R Mayúscula     | **114** | 72  | **r**  | r minúscula  |
| **51** | 33  | **3**  | Tres              | **83** | 53  | **S**  | S Mayúscula     | **115** | 73  | **s**  | s minúscula  |
| **52** | 34  | **4**  | Cuatro            | **84** | 54  | **T**  | T Mayúscula     | **116** | 74  | **t**  | t minúscula  |
| **53** | 35  | **5**  | Cinco             | **85** | 55  | **U**  | U Mayúscula     | **117** | 75  | **u**  | u minúscula  |
| **54** | 36  | **6**  | Seis              | **86** | 56  | **V**  | V Mayúscula     | **118** | 76  | **v**  | v minúscula  |
| **55** | 37  | **7**  | Siete             | **87** | 57  | **W**  | W Mayúscula     | **119** | 77  | **w**  | w minúscula  |
| **56** | 38  | **8**  | Ocho              | **88** | 58  | **X**  | X Mayúscula     | **120** | 78  | **x**  | x minúscula  |
| **57** | 39  | **9**  | Nueve             | **89** | 59  | **Y**  | Y Mayúscula     | **121** | 79  | **y**  | y minúscula  |
| **58** | 3A  | **:**  | Dos puntos        | **90** | 5A  | **Z**  | Z Mayúscula     | **122** | 7A  | **z**  | z minúscula  |
| **59** | 3B  | **;**  | Punto y coma      | **91** | 5B  | **[**  | Corchete abre   | **123** | 7B  | **{**  | Llave abre   |
| **60** | 3C  | **<**  | Menor que         | **92** | 5C  | **\\** | Barra inv.      | **124** | 7C  | **\|** | Tubería      |
| **61** | 3D  | **=**  | Igual             | **93** | 5D  | **]**  | Corchete cierra | **125** | 7D  | **}**  | Llave cierra |
| **62** | 3E  | **>**  | Mayor que         | **94** | 5E  | **^**  | Circunflejo     | **126** | 7E  | **~**  | Virgulilla   |
| **63** | 3F  | **?**  | Interrogación     | **95** | 5F  | **_**  | Guion bajo      |         |     |        |              |
  
3. **Bloque Latino-1 Suplementario (160 - 255)**

|   Dec   | Hex | Char  | Descripción             |   Dec   | Hex | Char  | Descripción         |   Dec   | Hex | Char  | Descripción   |
| :-----: | :-: | :---: | :---------------------- | :-----: | :-: | :---: | :------------------ | :-----: | :-: | :---: | :------------ |
| **160** | A0  |       | Espacio No-Breaking     | **192** | C0  | **À** | A grave Mayúscula   | **224** | E0  | **à** | a grave       |
| **161** | A1  | **¡** | Exclamación invertida   | **193** | C1  | **Á** | A aguda Mayúscula   | **225** | E1  | **á** | a aguda       |
| **162** | A2  | **¢** | Centavo                 | **194** | C2  | **Â** | A circunflejo Mayús | **226** | E2  | **â** | a circunflejo |
| **163** | A3  | **£** | Libra                   | **195** | C3  | **Ã** | A tilde Mayúscula   | **227** | E3  | **ã** | a tilde       |
| **164** | A4  | **¤** | Símbolo moneda          | **196** | C4  | **Ä** | A diéresis Mayús    | **228** | E4  | **ä** | a diéresis    |
| **165** | A5  | **¥** | Yen                     | **197** | C5  | **Å** | A con anillo Mayús  | **229** | E5  | **å** | a con anillo  |
| **166** | A6  | **¦** | Barra partida           | **198** | C6  | **Æ** | AE Mayúscula        | **230** | E6  | **æ** | ae            |
| **167** | A7  | **§** | Sección                 | **199** | C7  | **Ç** | C cedilla Mayúscula | **231** | E7  | **ç** | c cedilla     |
| **168** | A8  | **¨** | Diéresis                | **200** | C8  | **È** | E grave Mayúscula   | **232** | E8  | **è** | e grave       |
| **169** | A9  | **©** | Copyright               | **201** | C9  | **É** | E aguda Mayúscula   | **233** | E9  | **é** | e aguda       |
| **170** | AA  | **ª** | Ordinal Femenino        | **202** | CA  | **Ê** | E circunflejo Mayús | **234** | EA  | **ê** | e circunflejo |
| **171** | AB  | **«** | Comillas angulares izq  | **203** | CB  | **Ë** | E diéresis Mayús    | **235** | EB  | **ë** | e diéresis    |
| **172** | AC  | **¬** | Negación                | **204** | CC  | **Ì** | I grave Mayúscula   | **236** | EC  | **ì** | i grave       |
| **173** | AD  | **-** | Guion suave             | **205** | CD  | **Í** | I aguda Mayúscula   | **237** | ED  | **í** | i aguda       |
| **174** | AE  | **®** | Marca Registrada        | **206** | CE  | **Î** | I circunflejo Mayús | **238** | EE  | **î** | i circunflejo |
| **175** | AF  | **¯** | Macron                  | **207** | CF  | **Ï** | I diéresis Mayús    | **239** | EF  | **ï** | i diéresis    |
| **176** | B0  | **°** | Grado                   | **208** | D0  | **Ð** | Eth Mayúscula       | **240** | F0  | **ð** | eth           |
| **177** | B1  | **±** | Más menos               | **209** | D1  | **Ñ** | Eñe Mayúscula       | **241** | F1  | **ñ** | eñe           |
| **178** | B2  | **²** | Superíndice 2           | **210** | D2  | **Ò** | O grave Mayúscula   | **242** | F2  | **ò** | o grave       |
| **179** | B3  | **³** | Superíndice 3           | **211** | D3  | **Ó** | O aguda Mayúscula   | **243** | F3  | **ó** | o aguda       |
| **180** | B4  | **´** | Acento agudo            | **212** | D4  | **Ô** | O circunflejo Mayús | **244** | F4  | **ô** | o circunflejo |
| **181** | B5  | **µ** | Micro                   | **213** | D5  | **Õ** | O tilde Mayúscula   | **245** | F5  | **õ** | o tilde       |
| **182** | B6  | **¶** | Párrafo (Pilcrow)       | **214** | D6  | **Ö** | O diéresis Mayús    | **246** | F6  | **ö** | o diéresis    |
| **183** | B7  | **·** | Punto medio             | **215** | D7  | **×** | Multiplicación      | **247** | F7  | **÷** | División      |
| **184** | B8  | **¸** | Cedilla                 | **216** | D8  | **Ø** | O barrada Mayús     | **248** | F8  | **ø** | o barrada     |
| **185** | B9  | **¹** | Superíndice 1           | **217** | D9  | **Ù** | U grave Mayúscula   | **249** | F9  | **ù** | u grave       |
| **186** | BA  | **º** | Ordinal Masculino       | **218** | DA  | **Ú** | U aguda Mayúscula   | **250** | FA  | **ú** | u aguda       |
| **187** | BB  | **»** | Comillas angulares der  | **219** | DB  | **Û** | U circunflejo Mayús | **251** | FB  | **û** | u circunflejo |
| **188** | BC  | **¼** | Un cuarto               | **220** | DC  | **Ü** | U diéresis Mayús    | **252** | FC  | **ü** | u diéresis    |
| **189** | BD  | **½** | Un medio                | **221** | DD  | **Ý** | Y aguda Mayúscula   | **253** | FD  | **ý** | y aguda       |
| **190** | BE  | **¾** | Tres cuartos            | **222** | DE  | **Þ** | Thorn Mayúscula     | **254** | FE  | **þ** | thorn         |
| **191** | BF  | **¿** | Interrogación invertida | **223** | DF  | **ß** | Eszett (Alemana)    | **255** | FF  | **ÿ** | y diéresis    |

  
4. **Tabla Base64**

| Dec | Hex | Carácter | Dec | Hex | Carácter | Dec | Hex | Carácter | Dec | Hex | Carácter | Dec | Hex | Carácter |
| :-: | :-: | :------: | :-: | :-: | :------: | :-: | :-: | :------: | :-: | :-: | :------: | :-: | :-: | :------: |
|  0  | 00  |    A     | 13  | 0D  |    N     | 26  | 1A  |    a     | 39  | 27  |    n     | 52  | 34  |    0     |
|  1  | 01  |    B     | 14  | 0E  |    O     | 27  | 1B  |    b     | 40  | 28  |    o     | 53  | 35  |    1     |
|  2  | 02  |    C     | 15  | 0F  |    P     | 28  | 1C  |    c     | 41  | 29  |    p     | 54  | 36  |    2     |
|  3  | 03  |    D     | 16  | 10  |    Q     | 29  | 1D  |    d     | 42  | 2A  |    q     | 55  | 37  |    3     |
|  4  | 04  |    E     | 17  | 11  |    R     | 30  | 1E  |    e     | 43  | 2B  |    r     | 56  | 38  |    4     |
|  5  | 05  |    F     | 18  | 12  |    S     | 31  | 1F  |    f     | 44  | 2C  |    s     | 57  | 39  |    5     |
|  6  | 06  |    G     | 19  | 13  |    T     | 32  | 20  |    g     | 45  | 2D  |    t     | 58  | 3A  |    6     |
|  7  | 07  |    H     | 20  | 14  |    U     | 33  | 21  |    h     | 46  | 2E  |    u     | 59  | 3B  |    7     |
|  8  | 08  |    I     | 21  | 15  |    V     | 34  | 22  |    i     | 47  | 2F  |    v     | 60  | 3C  |    8     |
|  9  | 09  |    J     | 22  | 16  |    W     | 35  | 23  |    j     | 48  | 30  |    w     | 61  | 3D  |    9     |
| 10  | 0A  |    K     | 23  | 17  |    X     | 36  | 24  |    k     | 49  | 31  |    x     | 62  | 3E  |    +     |
| 11  | 0B  |    L     | 24  | 18  |    Y     | 37  | 25  |    l     | 50  | 32  |    y     | 63  | 3F  |    /     |
| 12  | 0C  |    M     | 25  | 19  |    Z     | 38  | 26  |    m     | 51  | 33  |    z     |     |     |          |

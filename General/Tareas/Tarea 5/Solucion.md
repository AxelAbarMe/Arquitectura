# TAREA 5 — Solución

> Convención usada en todos los mapas: filas **AB** (A = primer bit, B = segundo bit), columnas **CD** (C = primer bit, D = segundo bit), orden Gray 00,01,11,10. Las casillas en blanco de los mapas originales se interpretan como 0.

## 1. Implementa el circuito lógico utilizando la mínima cantidad de compuertas

### Mapa K #1

| AB \ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 1 | 0 | X | 1 |
| **01** | 1 | 1 | 1 | 1 |
| **11** | 1 | X | 0 | 1 |
| **10** | 1 | 0 | X | 1 |

**Agrupaciones:**
* Columnas `CD=00` y `CD=10` completas (las 4 filas; D=0, A y C libres) → **D'**
* Fila `AB=01` completa (A=0, B=1, C y D libres) → **A'B**

**F = D' + A'B**

```
A-->NOT----|
           -->AND--|
B----------|       |
                   --->OR--->F
D-->NOT-------------|
```
**Compuertas:** 2 NOT + 1 AND (2 entradas) + 1 OR (2 entradas) = **4 compuertas**.

---

### Mapa K #2

| AB \ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 0 | 0 | X | 1 |
| **01** | 1 | 1 | 1 | 1 |
| **11** | 1 | X | X | 1 |
| **10** | 0 | 0 | X | 1 |

**Agrupaciones:**
* Filas `AB=01` y `AB=11` (usando las X) → **B**
* Bloque `{(00,11),(00,10),(10,11),(10,10)}` (usando las X, B=0, C=1) → **B'C**

Como B + B'C = B + C (absorción), la expresión se reduce a:

**F = B + C**

```
B----------|
           --->OR--->F
C----------|
```
**Compuertas:** 1 OR (2 entradas) = **1 compuerta** (no se necesitan inversores).

---

### Mapa K #3

| AB \ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 0 | 0 | X | 0 |
| **01** | 1 | 1 | X | 1 |
| **11** | 0 | 0 | X | X |
| **10** | 1 | 1 | X | X |

**Agrupaciones:**
* Fila `AB=01` completa (usando la X) → **A'B**
* Fila `AB=10` completa (usando las X) → **AB'**

**F = A'B + AB'**

Este es el patrón "tablero de ajedrez" típico de la operación XOR (no se puede simplificar más desde el mapa K):

**F = A ⊕ B**

```
A----------|
           --XOR----->F
B----------|
```
**Compuertas:** 1 XOR = **1 compuerta**.

---

### Mapa K #4

| AB \ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 1 | 0 | X | 1 |
| **01** | 1 | 1 | 1 | 1 |
| **11** | 1 | X | X | 1 |
| **10** | 1 | 0 | X | 1 |

**Agrupaciones:**
* Columnas `CD=00` y `CD=10` completas (D=0) → **D'**
* Filas `AB=01` y `AB=11` (usando las X; B=1) → **B**

**F = D' + B**

```
D-->NOT----|
           --->OR--->F
B----------|
```
**Compuertas:** 1 NOT + 1 OR (2 entradas) = **2 compuertas**.

---

### Mapa K #5

| AB \ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 0 | 0 | X | 1 |
| **01** | X | 1 | 1 | 0 |
| **11** | X | X | X | 1 |
| **10** | 1 | 0 | 0 | X |

**Agrupaciones:**
* Bloque `{(10,00),(10,11),(11,00),(11,11)}` (usando X en 10,11 y 11,00; A=1, D=0) → **AD'**
* Bloque `{(01,01),(01,11),(11,01),(11,11)}` (usando X en 11,01 y 11,11; B=1, D=1) → **BD**
* Par `{(00,11),(00,01)}`... (usando X en 01,00; A=0, B=0, C=1) → **A'B'C**

**F = AD' + BD + A'B'C**

```
A----------|
           -->AND--|
D-->NOT----|       |
                   --->OR-|
B----------|       |      |
           -->AND--|      |
D----------|              --->OR--->F
                          |
A-->NOT----|              |
           -->AND--|      |
B-->NOT----|       -->AND-|
C----------|
```
**Compuertas:** 3 NOT + 3 AND (dos de 2 entradas, una de 3 entradas) + 1 OR (3 entradas) = **7 compuertas**.
(Se revisó factorización por literal común, pero ningún literal es común a los 3 términos, así que esta es la forma más económica.)

---

### Mapa K #6

| AB \ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | X | 0 | X | 1 |
| **01** | 1 | 1 | 1 | 0 |
| **11** | 0 | 1 | X | 1 |
| **10** | 1 | 1 | X | 1 |

**Agrupaciones (5 implicantes primos esenciales):**
* Bloque `{(10,00),(10,10),(00,00),(00,10)}` (usando X; B=0, D=0) → **B'D'**
* Bloque `{(10,01),(10,11),(11,01),(11,11)}` (usando X; A=1, D=1) → **AD**
* Bloque `{(10,10),(10,11),(11,10),(11,11)}` (usando X; A=1, C=1) → **AC**
* Par `{(01,00),(01,01)}` (A'=1, B=1, C'=1) → **A'BC'**
* Par `{(01,01),(01,11)}` (A'=1, B=1, D=1) → **A'BD**

Cada término es esencial (cada uno cubre al menos una casilla que ningún otro cubre), por lo que no se puede reducir a menos de 5 términos:

**F = B'D' + AD + AC + A'BC' + A'BD**

```
B-->NOT----|
           -->AND--|
D-->NOT----|       |
A----------|       |
           -->AND--|
D----------|       |
                   --->OR--->F
A----------|       |
           -->AND--|
C----------|       |
A-->NOT----|       |
           -->AND--|
B----------|       |
C-->NOT----|       |
                   -->AND--|
A-->NOT----|              |
           -->AND---------|
B----------|
D----------|
```
**Compuertas:** 4 NOT + 5 AND (tres de 2 entradas, dos de 3 entradas) + 1 OR (5 entradas) = **10 compuertas**.

---

### Mapa K #7

| AB \ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | X | 0 | X | 1 |
| **01** | 1 | 1 | 1 | 0 |
| **11** | 0 | 1 | X | 1 |
| **10** | X | X | 1 | X |

**Agrupaciones (5 implicantes primos esenciales):**
* Columna `CD=11` completa (usando las X; C=1, D=1) → **CD**
* Bloque `{(00,00),(00,10),(10,00),(10,10)}` (usando X; B=0, D=0) → **B'D'**
* Par `{(01,00),(01,01)}` (A'=1, B=1, C'=1) → **A'BC'**
* Bloque `{(10,01),(10,11),(11,01),(11,11)}` (usando X; A=1, D=1) → **AD**
* Bloque `{(10,10),(10,11),(11,10),(11,11)}` (usando X; A=1, C=1) → **AC**

**F = CD + B'D' + A'BC' + AD + AC**

**Compuertas:** 4 NOT (A',B',C',D') + 5 AND (cuatro de 2 entradas, una de 3 entradas) + 1 OR (5 entradas) = **10 compuertas**.

---

### Mapa K #8

| AB \ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | X | 0 | X | 1 |
| **01** | 1 | X | 1 | 0 |
| **11** | 0 | X | X | X |
| **10** | X | X | 1 | X |

**Agrupaciones:**
* Bloque `{(00,11),(00,10),(10,11),(10,10)}` (usando X; B=0, C=1) → **B'C**
* Columna `CD=11` completa (usando las X; C=1, D=1) → **CD**
* Par `{(01,00),(01,01)}` (usando X; A'=1, B=1, C'=1) → **A'BC'**

**F = B'C + CD + A'BC'**

```
B-->NOT----|
           -->AND--|
C----------|       |
                   --->OR--->F
C----------|       |
           -->AND--|
D----------|       |
                    |
A-->NOT----|        |
           -->AND---|
B----------|
C-->NOT----|
```
**Compuertas:** 3 NOT + 3 AND (dos de 2 entradas, una de 3 entradas) + 1 OR (3 entradas) = **7 compuertas**.
(B'C y CD comparten el literal C, pero factorizarlo solo entre 2 términos no reduce la cantidad de compuertas, así que se deja en SOP.)

---

### Mapa K #9

| AB \ CD | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | X | 0 | X | 1 |
| **01** | 1 | X | 1 | 0 |
| **11** | 0 | X | X | 1 |
| **10** | 0 | 0 | X | 1 |

**Agrupaciones:**
* Par `{(01,00),(01,01)}` (usando X; A'=1, B=1, C'=1) → **A'BC'**
* Bloque `{(00,11),(00,10),(10,11),(10,10)}` (usando X; B=0, C=1) → **B'C**
* Bloque `{(10,11),(10,10),(11,11),(11,10)}` (usando X; A=1, C=1) → **AC**
* Columna `CD=11` completa (usando las X; C=1, D=1) → **CD**

Los términos B'C, AC y CD comparten el literal C (¡3 términos!), lo cual **sí** permite ahorrar una compuerta al factorizar:

C(B' + A + D), en vez de 3 compuertas AND separadas, se implementa con 1 OR (3 entradas) + 1 AND = 2 compuertas.

**F = A'BC' + C(A + B' + D)**

```
A----------|
           |
B-->NOT----+--->OR--|
           |         -->AND--|
D----------|                 |
                             --->OR--->F
A-->NOT----|                 |
           -->AND--|         |
B----------|       -->AND----|
C-->NOT----|
```
**Compuertas:** 3 NOT (A',B',C') + 1 AND de 3 entradas (A'BC') + 1 OR de 3 entradas (A+B'+D) + 1 AND de 2 entradas (C · OR) + 1 OR final (2 entradas) = **7 compuertas** (en vez de 8 si se dejara como SOP plano de 4 términos).

---

## 2. Implementa el circuito lógico con compuerta de NAND utilizando los siguientes

### Mapa K #1 (X₁X₀ \ X₃X₂)

| X₁X₀ \ X₃X₂ | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 0 (m0) | 0 (m4) | X (m12) | 0 (m8) |
| **01** | 0 (m1) | 1 (m5) | X (m13) | 0 (m9) |
| **11** | 1 (m3) | 1 (m7) | X (m15) | X (m11) |
| **10** | 1 (m2) | 0 (m6) | X (m14) | X (m10) |

**Agrupaciones:**
* Fila `X1X0=11` completa (m3,7,15,11) → **X1X0**
* Bloque `{m2,m3,m10,m11}` (usando X; X2'=1, X1=1) → **X2'X1**
* Bloque `{m5,m7,m13,m15}` (usando X; X2=1, X0=1) → **X2X0**

**F = X1X0 + X2'X1 + X2X0**

Sólo se necesita un inversor (X2'); X1, X0 y X2 se usan tal cual.

```
X2 ----+---[--NAND ]---- X2'   (inversor)

X1 ----+
        [= NAND ]---- (X1X0)'
X0 ----+

X2'----+
        [= NAND ]---- (X2'X1)'
X1 ----+

X2 ----+
        [= NAND ]---- (X2X0)'
X0 ----+

(X1X0)'   ----+
(X2'X1)'  ----+---[= NAND ]---> F
(X2X0)'   ----+
```
**Compuertas NAND:** 1 inversor + 3 NAND de término + 1 NAND final = **5 compuertas**.

---

### Mapa K #2 (CD\AB)

| CD \ AB | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | X | 0 | X | 0 |
| **01** | 1 | 1 | X | 0 |
| **11** | 1 | 1 | X | X |
| **10** | 1 | 0 | X | X |

**Agrupaciones:**
* Bloque `{(01,00),(01,01),(11,00),(11,01)}` (A'=1, D=1) → **A'D**
* Bloque `{(01,00),(11,00),(10,00),(00,00)}` usando X en (00,00),(11,00),(10,00) — realmente el bloque `{m2,m3,m10,m11}` en binario ABCD: (B=0, C=1) → **B'C**

**F = A'D + B'C**

```
A ----+---[--NAND ]---- A'   (inversor)
B ----+---[--NAND ]---- B'   (inversor)

A'----+
       [= NAND ]---- (A'D)'
D ----+

B'----+
       [= NAND ]---- (B'C)'
C ----+

(A'D)' ----+
            [= NAND ]---> F
(B'C)' ----+
```
**Compuertas NAND:** 2 inversores + 2 NAND de término + 1 NAND final = **5 compuertas**.

---

## 3. Ejemplos de examen (No es para entregar)

### 3.1 F(w,x,y,z) = Σ(3,7,8,9,10,11,15), d(w,x,y,z) = Σ(0,2,5)

| wx \ yz | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **w'x'** | X | 0 | 1 | X |
| **w'x** | 0 | X | 1 | 0 |
| **wx** | 0 | 0 | 1 | 0 |
| **wx'** | 1 | 1 | 1 | 1 |

**Agrupaciones:**
* Fila `wx=10` completa (w=1, x=0) → **wx'**
* Columna `yz=11` completa (y=1, z=1) → **yz**

**F(w,x,y,z) = wx' + yz**

```
W----------|
           -->AND--|
X-->NOT----|       |
                   --->OR--->F
Y----------|       |
           -->AND--|
Z----------|
```
**Compuertas:** 1 NOT + 2 AND (2 entradas) + 1 OR (2 entradas) = **4 compuertas**.

---

### 3.2 F(w,x,y,z) = Σ(1,3,7,15), d(w,x,y,z) = Σ(0,2,5)

| wx \ yz | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **w'x'** | X | 1 | 1 | X |
| **w'x** | 0 | X | 1 | 0 |
| **wx** | 0 | 0 | 1 | 0 |
| **wx'** | 0 | 0 | 0 | 0 |

**Agrupaciones:**
* Fila `wx=00` completa (usando las X; w=0, x=0) → **w'x'**
* Bloque `{(w'x,11),(wx,11)}` (x=1, y=1, z=1) → **xyz**

**F(w,x,y,z) = w'x' + xyz**

```
W-->NOT----|
           -->AND--|
X-->NOT----|       |
                   --->OR--->F
X----------|       |
Y----------|       -->AND--|
Z----------|---------------|
```
**Compuertas:** 2 NOT + 2 AND (una de 2 entradas, una de 3 entradas) + 1 OR (2 entradas) = **5 compuertas**.

---

### 3.3 F(w,x,y,z) = Σ(1,6,7,11), d(w,x,y,z) = Σ(0,2,3,4,5,8,10,12)

| wx \ yz | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **w'x'** | X | 1 | X | X |
| **w'x** | X | X | 1 | 1 |
| **wx** | X | 0 | 0 | 0 |
| **wx'** | X | 0 | 1 | X |

**Agrupaciones:**
* Filas `wx=00` y `wx=01` completas (usando las X; w=0) → **w'**
* Bloque `{(w'x',11),(w'x',10),(wx',11),(wx',10)}` (usando las X; x=0, y=1) → **x'y**

Con tantos "no importa" disponibles, el grupo de 8 celdas (w') deja solo la celda 11 (fila wx', columna 11) por cubrir, cubierta por x'y:

**F(w,x,y,z) = w' + x'y**

```
W-->NOT----|
           --->OR--->F
X-->NOT----|
           -->AND--|
Y----------|       |
                    (entra a la OR de arriba)
```

Circuito completo:
```
X-->NOT----|
           -->AND--|
Y----------|       |
                   --->OR--->F
W-->NOT-------------|
```
**Compuertas:** 2 NOT + 1 AND (2 entradas) + 1 OR (2 entradas) = **4 compuertas**.

# TAREA 4 — Solución

## 1. Ejemplo de Examen

> Implemente de manera óptima y mínima el circuito lógico de la función $F$ utilizando exclusivamente compuertas NAND.
> $d(D,C,B,A) = \Sigma(4,5,10,11,12)$
> $F(D,C,B,A) = \Sigma(0,7,8,13,14)$

### Mapa K (4 variables) — filas DC, columnas BA

| DC \ BA | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 1 | 0 | 0 | 0 |
| **01** | X | X | 1 | 0 |
| **11** | X | 1 | 0 | 1 |
| **10** | 1 | 0 | X | X |

### Agrupaciones

* Columna `BA=00` completa ($m_0, m_4, m_{12}, m_8$; usando $X$ en 4 y 12) $\rightarrow$ **$B'A'$**
* Bloque $\{4,5,12,13\}$ ($C=1, B=0$, usando $X$ en 4, 5 y 12) $\rightarrow$ **$CB'$**
* Par $\{5,7\}$ ($D=0, C=1, A=1$, usando $X$ en 5) $\rightarrow$ **$D'CA$**
* Bloque $\{8,10,12,14\}$ ($D=1, A=0$, usando $X$ en 10 y 12) $\rightarrow$ **$DA'$**

**$F(D,C,B,A) = B'A' + CB' + D'CA + DA'$**

### Circuito lógico (sólo compuertas NAND)

Se utiliza la estructura clásica NAND-NAND de dos niveles. En el primer nivel, los términos producto se generan mediante compuertas NAND (obteniendo cada término negado). En el segundo nivel, una compuerta NAND final combina todas las salidas para realizar la suma lógica por la Ley de De Morgan.

Inversores necesarios (NAND de una entrada duplicada):

```
A ----+---[ NAND ]---- A'
      |
      +---[ NAND ]
      
B ----+---[ NAND ]---- B'
      |
      +---[ NAND ]

D ----+---[ NAND ]---- D'
      |
      +---[ NAND ]

```

Términos del primer nivel:

```
B'----+
      |=== [ NAND ] ---- (B'A')'
A'----+

C ----+
      |=== [ NAND ] ---- (CB')'
B'----+

D'----+
C ----+=== [ NAND ] ---- (D'CA)'
A ----+

D ----+
      |=== [ NAND ] ---- (DA')'
A'----+

```

Etapa de combinación final (NAND de 4 entradas):

```
(B'A')'  ----+
(CB')'   ----+
             |=== [ NAND ] ---> F
(D'CA)'  ----+
(DA')'   ----+

```

---

## 2. Obtenga / Simplifique e implemente la función

**$F(A,B,C,D) = A'B'C' + B'CD' + A'BCD' + AB'C'$**

### Obtención de minitérminos

* $A'B'C'$ ($D$ libre) $\rightarrow m_0, m_1$
* $B'CD'$ ($A$ libre) $\rightarrow m_2, m_{10}$
* $A'BCD'$ $\rightarrow m_6$
* $AB'C'$ ($D$ libre) $\rightarrow m_8, m_9$

$$F(A,B,C,D) = \Sigma(0,1,2,6,8,9,10)$$

### Mapa K — filas AB, columnas CD

| AB \ CD | 00 ($C'D'$) | 01 ($C'D$) | 11 ($CD$) | 10 ($CD'$) |
| --- | --- | --- | --- | --- |
| **00 ($A'B'$)** | 1 | 1 | 0 | 1 |
| **01 ($A'B$)** | 0 | 0 | 0 | 1 |
| **11 ($AB$)** | 0 | 0 | 0 | 0 |
| **10 ($AB'$)** | 1 | 1 | 0 | 1 |

### Agrupaciones

* Grupo de 4 esquinas $\{0,2,8,10\}$ (adyacencia entre esquinas; $B=0, D=0$) $\rightarrow$ **$B'D'$**
* Bloque de 4 $\{0,1,8,9\}$ ($B=0, C=0$) $\rightarrow$ **$B'C'$**
* Par $\{2,6\}$ ($A=0, C=1, D=0$) $\rightarrow$ **$A'CD'$**

**$F(A,B,C,D) = B'D' + B'C' + A'CD'$**

*(Todos son implicantes primos esenciales: $m_{10}$ solo es cubierto por $B'D'$, $m_9$ solo por $B'C'$, y $m_6$ solo por $A'CD'$).*

### Circuito lógico (compuertas básicas AND/OR/NOT)

```
B ---> [ NOT ] ---+
                  |== [ AND ] ---+
D ---> [ NOT ] ---+              |
                                 |== [ OR ] ---+
B ---> [ NOT ] ---+              |             |
                  |== [ AND ] ---+             |
C ---> [ NOT ] ---+                            |=== [ OR ] ---> F
                                               |
A ---> [ NOT ] ---+                            |
C ----------------|== [ AND ] -----------------+
D ---> [ NOT ] ---+

```

---

## 3. Obtenga / Simplifique e implemente las siguientes funciones

### 3.1 $F(x, y, z) = \Sigma(1, 3, 7)$

| x \ yz | 00 ($y'z'$) | 01 ($y'z$) | 11 ($yz$) | 10 ($yz'$) |
| --- | --- | --- | --- | --- |
| **0 ($x'$)** | 0 | 1 | 1 | 0 |
| **1 ($x$)** | 0 | 0 | 1 | 0 |

**Agrupaciones:**

* Par $\{3,7\}$ ($y=1, z=1$) $\rightarrow$ **$yz$**
* Par $\{1,3\}$ ($x=0, z=1$) $\rightarrow$ **$x'z$**

**$F(x,y,z) = x'z + yz$**

Factorizando la variable común $z$: **$F = z(x' + y)$**.

```
X ---> [ NOT ] ---+
                  |=== [ OR ] ---+
Y ----------------+              |=== [ AND ] ---> F
                                 |
Z -------------------------------+

```

**Compuertas requeridas:** 1 NOT + 1 OR (2 entradas) + 1 AND (2 entradas) = 3 compuertas.

---

### 3.2 $F(x, y, z) = \Sigma(0, 1, 2, 6)$

| x \ yz | 00 ($y'z'$) | 01 ($y'z$) | 11 ($yz$) | 10 ($yz'$) |
| --- | --- | --- | --- | --- |
| **0 ($x'$)** | 1 | 1 | 0 | 1 |
| **1 ($x$)** | 0 | 0 | 0 | 1 |

**Agrupaciones:**

* Par $\{0,1\}$ ($x=0, y=0$) $\rightarrow$ **$x'y'$**
* Par $\{2,6\}$ ($y=1, z=0$) $\rightarrow$ **$yz'$**

**$F(x,y,z) = x'y' + yz'$**

```
X ---> [ NOT ] ---+
                  |=== [ AND ] ---+
Y ---> [ NOT ] ---+               |
                                  |=== [ OR ] ---> F
Y ----------------+               |
                  |=== [ AND ] ---+
Z ---> [ NOT ] ---+

```

---

### 3.3 $F(a, b, c) = \Sigma(0, 1, 2, 3, 4, 5)$

| a \ bc | 00 ($b'c'$) | 01 ($b'c$) | 11 ($bc$) | 10 ($bc'$) |
| --- | --- | --- | --- | --- |
| **0 ($a'$)** | 1 | 1 | 1 | 1 |
| **1 ($a$)** | 1 | 1 | 0 | 0 |

**Agrupaciones:**

* Fila $a'$ completa ($\{0,1,2,3\}$) $\rightarrow$ **$a'$**
* Bloque de 4 en las dos primeras columnas ($\{0,1,4,5\}$) $\rightarrow$ **$b'$**

**$F(a,b,c) = a' + b'$**

```
A ---> [ NOT ] ---+
                  |=== [ OR ] ---> F
B ---> [ NOT ] ---+

```

---

### 3.4 $F(w, x, y, z) = \Sigma(0, 1, 2, 4, 5, 6, 8, 9, 12, 13, 14)$

| wx \ yz | 00 ($y'z'$) | 01 ($y'z$) | 11 ($yz$) | 10 ($yz'$) |
| --- | --- | --- | --- | --- |
| **00 ($w'x'$)** | 1 | 1 | 0 | 1 |
| **01 ($w'x$)** | 1 | 1 | 0 | 1 |
| **11 ($wx$)** | 1 | 1 | 0 | 1 |
| **10 ($wx'$)** | 1 | 1 | 0 | 0 |

**Agrupaciones:**

* Bloque de 8 (columnas $y'z'$ y $y'z$ completas) $\rightarrow$ **$y'$**
* Bloque de 4 ($\{4,6,12,14\}$; $x=1, z=0$) $\rightarrow$ **$xz'$**
* Par de esquinas superior/inferior ($\{0,2,8\}$; sin el 10 disponible, se agrupan $m_0$ y $m_2$ con $m_8$, resultando en la agrupación de $m_0, m_2$) $\rightarrow$ **$w'x'z'$**

**$F(w,x,y,z) = y' + xz' + w'x'z'$**

```
Y ---> [ NOT ] -----------------------------------+
                                                  |
X ----------------+                               |
                  |=== [ AND ] -------------------+
Z ---> [ NOT ] ---+                               |
                                                  |=== [ OR ] ---> F
W ---> [ NOT ] ---+                               |
X ---> [ NOT ] ---|=== [ AND ] -------------------+
Z ---> [ NOT ] ---+

```

---

### 3.5 $F = A'B'C' + B'CD' + A'BCD' + AB'C'$

*(Corresponde a la misma función analizada en el numeral 2).*

**$F(A,B,C,D) = B'D' + B'C' + A'CD'$**

---

### Implemente el circuito lógico utilizando únicamente compuertas NAND:

#### Mapa K #3 (CD \ AB)

| CD \ AB | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 1 | 0 | 0 | 1 |
| **01** | 1 | 1 | 1 | 1 |
| **11** | 1 | 0 | 0 | 1 |
| **10** | 1 | 0 | 0 | 1 |

**Agrupaciones:**

* Columna `AB=00` completa $\rightarrow$ **$A'B'$**
* Columna `AB=10` completa $\rightarrow$ **$AB'$**
* Fila `CD=01` completa $\rightarrow$ **$C'D$**

Sumando las columnas: $A'B' + AB' = B'(A' + A) = B'$.

**$F = B' + C'D$**

##### Circuito NAND

1. Generar la señal $B'$:

```
B ----+---[ NAND ]---- B'
      |
      +---[ NAND ]

```

2. Generar el término $(C'D)'$:

```
C ----+---[ NAND ]---- C'
      |
      +---[ NAND ]

C'----+
      |=== [ NAND ] ---- (C'D)'
D ----+

```

3. Combinación en la compuerta NAND final:

Para combinar términos simples con la etapa NAND final bajo la transformación OR-NAND, la señal $B'$ entra como entrada negada $(B')' = B$ para compensar la inversión implícita del nivel de entrada.

```
B  -----------------------+
                          |=== [ NAND ] ---> F
(C'D)' -------------------+

```

---

#### Mapa K #4 (CD \ AB)

| CD \ AB | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 1 | 0 | 0 | 0 |
| **01** | 1 | 1 | 1 | 0 |
| **11** | 1 | 1 | 0 | 1 |
| **10** | 1 | 0 | 0 | 0 |

**Agrupaciones:**

* Columna `AB=00` completa $\rightarrow$ **$A'B'$**
* Bloque $\{4,5,12,13\}$ ($D=1, A=0$) $\rightarrow$ **$A'D$**
* Par $\{5,7\}$ ($C=0, D=1, B=1$) $\rightarrow$ **$BC'D$**
* Par $\{12,14\}$ ($C=1, D=1, B=0$) $\rightarrow$ **$B'CD$**

**$F = A'B' + A'D + BC'D + B'CD$**

##### Circuito NAND

Inversores:

```
A ----+---[ NAND ]---- A'
      |
      +---[ NAND ]

B ----+---[ NAND ]---- B'
      |
      +---[ NAND ]

C ----+---[ NAND ]---- C'
      |
      +---[ NAND ]

```

Términos del primer nivel:

```
A'----+
      |=== [ NAND ] ---- (A'B')'
B'----+

A'----+
      |=== [ NAND ] ---- (A'D)'
D ----+

B ----+
C'----+=== [ NAND ] ---- (BC'D)'
D ----+

B'----+
C ----+=== [ NAND ] ---- (B'CD)'
D ----+

```

Compuerta final:

```
(A'B')'  ----+
(A'D)'   ----+
             |=== [ NAND ] ---> F
(BC'D)'  ----+
(B'CD)'  ----+

```

---

### Implemente el circuito lógico utilizando únicamente compuertas NAND:

#### Mapa K #1 (CD \ AB)

| CD \ AB | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 1 | 0 | 1 | 1 |
| **01** | 0 | 1 | 1 | 1 |
| **11** | 0 | 1 | 1 | 0 |
| **10** | 1 | 0 | X | 1 |

**Agrupaciones:**

* Columna `AB=11` completa (usando $X$ en $CD=10$) $\rightarrow$ **$AB$**
* Par $\{0,8\}$ ($D=0, A=0, B=0$) $\rightarrow$ **$A'B'D'$**
* Par $\{5,13\}$ ($D=1, A=0, B=1$) $\rightarrow$ **$A'BD$**
* Par $\{2,6\}$ ($C=0, A=1, B=0$) $\rightarrow$ **$AB'C'$**
* Par $\{2,10\}$ ($D=0, A=1, B=0$) $\rightarrow$ **$AB'D'$**

**$F = AB + A'B'D' + A'BD + AB'C' + AB'D'$**

##### Circuito NAND

Inversores:

```
A ----+---[ NAND ]---- A'
      |
      +---[ NAND ]

B ----+---[ NAND ]---- B'
      |
      +---[ NAND ]

C ----+---[ NAND ]---- C'
      |
      +---[ NAND ]

D ----+---[ NAND ]---- D'
      |
      +---[ NAND ]

```

Términos del primer nivel:

```
A ----+
      |=== [ NAND ] ---- (AB)'
B ----+

A'----+
B'----+=== [ NAND ] ---- (A'B'D')'
D'----+

A'----+
B ----+=== [ NAND ] ---- (A'BD)'
D ----+

A ----+
B'----+=== [ NAND ] ---- (AB'C')'
C'----+

A ----+
B'----+=== [ NAND ] ---- (AB'D')'
D'----+

```

Compuerta final:

```
(AB)'      ----+
(A'B'D')'  ----+
(A'BD)'    ----+=== [ NAND ] ---> F
(AB'C')'   ----+
(AB'D')'   ----+

```

---

#### Mapa K #2 (CD \ AB)

| CD \ AB | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 1 | 0 | 0 | 1 |
| **01** | 1 | 1 | 1 | 1 |
| **11** | 1 | 1 | 1 | 1 |
| **10** | 1 | 0 | X | 1 |

**Agrupaciones:**

* Filas `CD=01` y `CD=11` completas ($D=1$) $\rightarrow$ **$D$**
* Bloque de 4 esquinas $\{0,2,8,10\}$ ($D=0, B=0$, usando $X$ en $CD=10, AB=11$) $\rightarrow$ **$D'B'$**

Por absorción booleana ($D + D'B' = D + B'$):

**$F = D + B'$**

##### Circuito NAND

Para implementar una función en forma de suma simple con una compuerta NAND, aplicamos la identidad de De Morgan: $F = D + B' = ((D + B')')' = (D' \cdot B) '$.

Las entradas requeridas para la compuerta NAND final son las señales invertidas $D'$ y $B$:

```
D ----+---[ NAND ]---- D'
      |
      +---[ NAND ]

D' -----------------------+
                          |=== [ NAND ] ---> F
B  -----------------------+

```

---

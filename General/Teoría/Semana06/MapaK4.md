# Mapa Karnaugh de 4 Variables

## Forma base del Mapa K (Template)

Un mapa de Karnaugh se construye a partir de una tabla de verdad, agrupando minitérminos adyacentes (donde solo cambia **un bit**) para eliminar variables.

* **Grupo de 2 celdas:** elimina **1 variable**.
* **Grupo de 4 celdas:** elimina **2 variables**.
* **Grupo de 8 celdas:** elimina **3 variables**.
* (Un grupo de 16 celdas —el mapa completo— eliminaría las 4 variables, dando F=1 siempre.)

> Entre más grande sea el grupo, menos variables (literales) tendrá el término resultante.

### Plantilla base (4 variables)

Usando como ejemplo las variables W (peso 8), X (peso 4), Y (peso 2), Z (peso 1):

| \ | YZ | Y'Z' | Y'Z | YZ | YZ' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| WX | \ | **00** | **01** | **11** | **10** |
| W'X' | **00** | 0 | 1 | 3 | 2 |
| W'X | **01** | 4 | 5 | 7 | 6 |
| WX | **11** | 12 | 13 | 15 | 14 |
| WX' | **10** | 8 | 9 | 11 | 10 |

> Tanto filas como columnas siguen el **código Gray** (00,01,11,10) para garantizar que las celdas adyacentes (incluyendo los bordes, que se envuelven/*wraparound*) difieran en un solo bit.

En los siguientes ejercicios, se usan las **dos primeras variables** dadas como filas, y las **dos últimas** como columnas, respetando el orden indicado en cada enunciado (la primera variable listada es el bit más significativo).

---

## Ejercicio 1

> F(x,y,z,w) = Σm(0,1,3,5,6,7,8,11,15)

| \ | zw | z'w' | z'w | zw | zw' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| xy | \ | **00** | **01** | **11** | **10** |
| x'y' | **00** | 1 | 1 | 1 | 0 |
| x'y | **01** | 0 | 1 | 1 | 1 |
| xy | **11** | 0 | 0 | 1 | 0 |
| xy' | **10** | 1 | 0 | 1 | 0 |

**Agrupaciones:**
* Columna `zw=11` completa (filas 0,1,3,7,15,11) → **zw**
* Bloque `{1,3,5,7}` (x'=0, w=1, y,z libres) → **x'w**
* Par `{0,8}` (y=0,z=0,w=0, x libre) → **y'z'w'**
* Par `{6,7}` (x=0,y=1,z=1, w libre) → **x'yz**

**F(x,y,z,w) = x'w + zw + y'z'w' + x'yz**

---

## Ejercicio 2

> F(A,B,C,D) = Σm(0,2,3,4,5,6,8,9,10,11,15)

| \ | CD | C'D' | C'D | CD | CD' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| AB | \ | **00** | **01** | **11** | **10** |
| A'B' | **00** | 1 | 0 | 1 | 1 |
| A'B | **01** | 1 | 1 | 0 | 1 |
| AB | **11** | 0 | 0 | 1 | 0 |
| AB' | **10** | 1 | 1 | 1 | 1 |

**Agrupaciones:**
* Fila `AB'` completa (8,9,10,11) → **AB'**
* Bloque `{0,2,4,6}` (A=0,D=0, B,C libres) → **A'D'**
* Par `{4,5}` (A=0,B=1,C=0, D libre) → **A'BC'**
* Par `{11,15}` (A=1,C=1,D=1, B libre) → **ACD**
* Par `{3,11}` (B=0,C=1,D=1, A libre) → **B'CD**

**F(A,B,C,D) = AB' + A'D' + A'BC' + ACD + B'CD**

---

## Ejercicio 3

> F(A,B,C,D) = Σm(0,1,3,4,9,10,11,15)

| \ | CD | C'D' | C'D | CD | CD' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| AB | \ | **00** | **01** | **11** | **10** |
| A'B' | **00** | 1 | 1 | 1 | 0 |
| A'B | **01** | 1 | 0 | 0 | 0 |
| AB | **11** | 0 | 0 | 1 | 0 |
| AB' | **10** | 0 | 1 | 1 | 1 |

**Agrupaciones:**
* Bloque `{1,3,9,11}` (B=0,D=1, A,C libres) → **B'D**
* Par `{10,11}` (A=1,B=0,C=1, D libre) → **AB'C**
* Par `{0,4}` (A=0,C=0,D=0, B libre) → **A'C'D'**
* Par `{11,15}` (A=1,C=1,D=1, B libre) → **ACD**

**F(A,B,C,D) = AB'C + B'D + A'C'D' + ACD**

---

## Ejercicio 4

> F(D,C,B,A) = Σm(0,1,2,4,6,9,10,12,14)

| \ | BA | B'A' | B'A | BA | BA' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| DC | \ | **00** | **01** | **11** | **10** |
| D'C' | **00** | 1 | 1 | 0 | 1 |
| D'C | **01** | 1 | 0 | 0 | 1 |
| DC | **11** | 1 | 0 | 0 | 1 |
| DC' | **10** | 0 | 1 | 0 | 1 |

**Agrupaciones:**
* Columna `BA=10` completa (2,6,14,10) → **BA'**
* Bloque `{4,6,12,14}` (C=1,A=0, D,B libres) → **CA'**
* Bloque `{0,2,4,6}` (D=0,A=0, C,B libres) → **D'A'**
* Par `{1,9}` (C=0,B=0,A=1, D libre) → **C'B'A**

**F(D,C,B,A) = BA' + CA' + D'A' + C'B'A**

---

## Ejercicio 5

> F(x,y,z,w) = Σm(0,1,2,5,7,9,10,13,14,15)

| \ | zw | z'w' | z'w | zw | zw' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| xy | \ | **00** | **01** | **11** | **10** |
| x'y' | **00** | 1 | 1 | 0 | 1 |
| x'y | **01** | 0 | 1 | 1 | 0 |
| xy | **11** | 0 | 1 | 1 | 1 |
| xy' | **10** | 0 | 1 | 0 | 1 |

**Agrupaciones:**
* Columna `zw=01` completa (1,5,13,9) → **z'w**
* Par `{0,2}` (x=0,y=0,w=0, z libre) → **x'y'w'**
* Par `{10,14}` (x=1,z=1,w=0, y libre) → **xzw'**
* Par `{7,15}` (y=1,z=1,w=1, x libre) → **yzw**

**F(x,y,z,w) = z'w + x'y'w' + xzw' + yzw**

---

## Ejercicio 6

> F(A,B,C,D) = Σm(0,1,2,3,4,6,8,9,10,11,13,14)

| \ | CD | C'D' | C'D | CD | CD' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| AB | \ | **00** | **01** | **11** | **10** |
| A'B' | **00** | 1 | 1 | 1 | 1 |
| A'B | **01** | 1 | 0 | 0 | 1 |
| AB | **11** | 0 | 1 | 0 | 1 |
| AB' | **10** | 1 | 1 | 1 | 1 |

**Agrupaciones:**
* Filas `A'B'` y `AB'` combinadas (ambas B=0) → grupo de **8 celdas** → **B'**
* Bloque `{4,6,12,14}`... revisando: `{4,6,13,14}`? no —bloque real: {4,6}∪{13}... el correcto: par `{4,6}` no maximal; el grupo válido es `{2,6,10,14}` (C=1,D=0, A,B libres) → **CD'**

  *(Nota: aquí conviene ver que ya con B' cubrimos 8 celdas; falta cubrir 4,6,13,14)*
* Bloque `{0,2,4,6}` (A=0,D=0) → **A'D'**
* Bloque `{2,6,10,14}` (C=1,D=0) → **CD'**
* Par `{9,13}` (A=1,C=0,D=1, B libre) → **AC'D**

**F(A,B,C,D) = B' + A'D' + CD' + AC'D**

---

## Ejercicio 7

> F(x,y,z,w) = Σm(0,1,3,5,6,7,8,11,15)

> Es **idéntico al Ejercicio 1** (misma expresión y mismos minitérminos).

**F(x,y,z,w) = x'w + zw + y'z'w' + x'yz**

---
---

# El "No importa" (Don't Care)

## ¿Qué es?

Es una celda del mapa K cuya combinación de variables **nunca ocurre** en la práctica (o cuyo resultado es indiferente para el diseño). Se marca con una **X** dentro de la tabla.

* Una X **puede tratarse como 1** si esto ayuda a formar un grupo más grande (reduciendo variables).
* Una X **puede tratarse como 0** (simplemente ignorarse) si no aporta ningún beneficio.
* **Nunca es obligatorio** usarla; solo se usa cuando conviene a la simplificación.

## Ejemplo donde SÍ sirve (3 variables)

> F(x,y,z) = Σm(0,2,4,6) — nota: si el 4 fuera un don't care en vez de minitérmino real, veamos el efecto:

Suponga F(x,y,z) = Σm(0,2,6) + d(4)

| \ | yz | y'z' | y'z | yz | yz' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| x | \ | **00** | **01** | **11** | **10** |
| x' | **0** | 1 | 0 | 0 | 1 |
| x | **1** | X | 0 | 0 | 1 |

* **Sin usar el don't care:** se necesitarían dos pares: `{0,2}=x'z'` y `{2,6}=yz'`, dando F = x'z' + yz' (2 términos).
* **Usando el 4 como 1:** las celdas 0,2,4,6 comparten `z=0`, formando un solo grupo de 4 → **F = z'** (1 término, mucho más simple).

> Aquí el don't care **sí ayuda**, porque es adyacente a minitérminos reales y permite formar un grupo mayor.

## Ejemplo donde NO sirve (3 variables)

> F(x,y,z) = Σm(0,3,5) + d(6)

| \ | yz | y'z' | y'z | yz | yz' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| x | \ | **00** | **01** | **11** | **10** |
| x' | **0** | 1 | 0 | 1 | 0 |
| x | **1** | 0 | 1 | 0 | X |

* El 0, el 3 y el 5 están completamente aislados entre sí (ninguno es adyacente a otro minitérmino real).
* El don't care (6) tampoco es adyacente a ninguno de ellos (0,3,5).
* **Usar el 6 no reduce nada**: cada minitérmino real sigue necesitando su propio término de 3 literales (x'y'z' + x'yz + xy'z). El don't care simplemente se ignora (se deja en 0).

## Ejemplo en 4 variables

El caso más claro de esta práctica es el **Ejercicio 7 de don't care** resuelto abajo: gracias a la gran cantidad de X disponibles, una función con solo 5 minitérminos reales termina simplificándose a únicamente **2 términos** (`BA + DB'`), algo que sería imposible sin aprovechar los don't cares. En contraste, en varios de los otros ejercicios (por ejemplo, el 5), algunos minitérminos reales (como el 6) solo tienen **un** vecino disponible entre los don't cares, y aun así el grupo resultante sigue siendo pequeño (par), mostrando que el don't care ayuda, pero no siempre garantiza una reducción drástica — depende de la posición relativa de las X frente a los minitérminos reales.

---

## Don't Care 1

> F(D,C,B,A) = Σm(0,1,3,5,7,8,9,14). d(4,6,10,13,15)

| \ | BA | B'A' | B'A | BA | BA' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| DC | \ | **00** | **01** | **11** | **10** |
| D'C' | **00** | 1 | 1 | 1 | 0 |
| D'C | **01** | X | 1 | 1 | X |
| DC | **11** | 0 | X | X | 1 |
| DC' | **10** | 1 | 1 | 0 | X |

**Agrupaciones:**
* Bloque `{1,3,5,7}` (D=0, A=1, B,C libres) → **D'A**
* Bloque `{0,1,8,9}` (C=0,B=0, D,A libres) → **C'B'**
* Bloque `{6,7,14,15}` (usando 6 y 15 como X; C=1,B=1, D,A libres) → **CB**

**F(D,C,B,A) = D'A + C'B' + CB**

---

## Don't Care 2

> F(D,C,B,A) = Σm(0,2,3,4,5,6,7,11,13,14). d(8,10,15)

| \ | BA | B'A' | B'A | BA | BA' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| DC | \ | **00** | **01** | **11** | **10** |
| D'C' | **00** | 1 | 0 | 1 | 1 |
| D'C | **01** | 1 | 1 | 1 | 1 |
| DC | **11** | 0 | 1 | X | 1 |
| DC' | **10** | X | 0 | 1 | X |

**Agrupaciones:**
* Fila `D'C` completa (4,5,6,7) → **D'C**
* Columna `B=1` completa usando X (2,3,6,7,10,11,14,15) → **B**
* Bloque `{0,2,8,10}` (C=0,A=0, usando X en 8,10) → **C'A'**
* Bloque `{5,7,13,15}` (C=1,A=1, usando X en 15) → **CA**

**F(D,C,B,A) = D'C + B + C'A' + CA**

---

## Don't Care 3

> F(D,C,B,A) = Σm(0,3,6,7,9,13,15). d(2,5,8,10,11,12)

| \ | BA | B'A' | B'A | BA | BA' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| DC | \ | **00** | **01** | **11** | **10** |
| D'C' | **00** | 1 | 0 | 1 | X |
| D'C | **01** | 0 | X | 1 | 1 |
| DC | **11** | X | 1 | 1 | 0 |
| DC' | **10** | X | 1 | X | X |

**Agrupaciones:**
* Columna `BA=11` completa (usando 11,15 como X) → **BA**
* Bloque `{9,11,13,15}` (D=1,A=1, usando X en 11,15) → **DA**
* Bloque `{0,2,8,10}` (C=0,A=0, usando X en 2,8,10) → **C'A'**
* Par `{6,7}` (D=0,C=1,B=1, A libre) → **D'CB**

**F(D,C,B,A) = BA + DA + C'A' + D'CB**

---

## Don't Care 4

> F(D,C,B,A) = Σm(0,3,5,7,10,11,15). d(4,8,9,13,14)

| \ | BA | B'A' | B'A | BA | BA' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| DC | \ | **00** | **01** | **11** | **10** |
| D'C' | **00** | 1 | 0 | 1 | 0 |
| D'C | **01** | X | 1 | 1 | 0 |
| DC | **11** | 0 | X | 1 | X |
| DC' | **10** | X | X | 1 | 1 |

**Agrupaciones:**
* Columna `BA=11` completa (3,7,15,11, todos reales) → **BA**
* Bloque `{5,7,13,15}` (C=1,A=1, usando X en 13,15) → **CA**
* Fila `DC'` completa (usando X en 8,9; 10,11 reales) → **DC'**
* Par `{0,8}` (C=0,B=0,A=0, D libre, usando X en 8) → **C'B'A'**

**F(D,C,B,A) = BA + CA + DC' + C'B'A'**

---

## Don't Care 5

> F(D,C,B,A) = Σm(1,2,6,7,9,10,11,13,14). d(0,4,8,12)

| \ | BA | B'A' | B'A | BA | BA' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| DC | \ | **00** | **01** | **11** | **10** |
| D'C' | **00** | X | 1 | 0 | 1 |
| D'C | **01** | X | 0 | 1 | 1 |
| DC | **11** | X | 1 | 0 | 1 |
| DC' | **10** | X | 1 | 1 | 1 |

**Agrupaciones:**
* Columna `BA=10` completa (2,6,14,10) → **BA'**
* Bloque `{8,9,12,13}` (D=1,B=0, usando X en 8,12) → **DB'**
* Bloque `{0,1,8,9}` (C=0,B=0, usando X en 0,8) → **C'B'**
* Par `{6,7}` (D=0,C=1,B=1, A libre) → **D'CB**
* Par `{9,11}` (D=1,C=0,A=1, B libre) → **DC'A**

**F(D,C,B,A) = BA' + DB' + C'B' + D'CB + DC'A**

---

## Don't Care 6

> F(D,C,B,A) = Σm(2,5,7,8,9,11,13). d(0,3,6,10,14)

| \ | BA | B'A' | B'A | BA | BA' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| DC | \ | **00** | **01** | **11** | **10** |
| D'C' | **00** | X | 0 | X | 1 |
| D'C | **01** | 0 | 1 | 1 | X |
| DC | **11** | 0 | 1 | 0 | X |
| DC' | **10** | 1 | 1 | 1 | X |

**Agrupaciones:**
* Columna `BA=10` completa (usando X en 6,10,14; 2 real) → **BA'**
* Fila `DC'` completa (8,9,11 reales; 10 X) → **DC'**
* Par `{5,13}` (C=1,B=0,A=1, D libre) → **CB'A**
* Par `{3,7}` (D=0,B=1,A=1, C libre, usando X en 3) → **D'BA**

**F(D,C,B,A) = BA' + DC' + CB'A + D'BA**

---

## Don't Care 7

> F(D,C,B,A) = Σm(3,7,9,12,13). d(0,1,2,6,8,10,11,15)

| \ | BA | B'A' | B'A | BA | BA' |
|:--:|:--:|:--:|:--:|:--:|:--:|
| DC | \ | **00** | **01** | **11** | **10** |
| D'C' | **00** | X | X | 1 | X |
| D'C | **01** | 0 | 0 | 1 | X |
| DC | **11** | 1 | 1 | X | 0 |
| DC' | **10** | X | 1 | X | X |

**Agrupaciones:**
* Bloque `{3,7,11,15}` (B=1,A=1, usando X en 11,15) → **BA**
* Bloque `{8,9,12,13}` (D=1,B=0, usando X en 8) → **DB'**

> Gracias a la enorme cantidad de don't cares, ambos grupos de 4 celdas cubren **todos** los minitérminos requeridos con solo **2 términos** de 2 literales cada uno — el ejemplo más claro de cómo el "no importa" puede simplificar drásticamente una expresión.

**F(D,C,B,A) = BA + DB'**

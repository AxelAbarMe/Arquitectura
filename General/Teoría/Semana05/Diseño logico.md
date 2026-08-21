# Diseño Lógico

## Primer paso

Siempre se definen las variables de entrada y de salida.

| x | y | F |
|:---:|:---:|:---:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Depende de la cantidad de variables: se tienen `2^x` combinaciones posibles, donde `x` es la cantidad de variables.

> Las tablas de verdad permiten revisar todas las posibles combinaciones.

---

## Minitérmino

Un **minitérmino** es un término booleano donde participan las variables en su forma normal o complementada (`x` y `x'`).

| x | y' | z | m5 |
|:---:|:---:|:---:|:---:|
| 1 | 0 | 1 | |

* Si la variable aparece en su forma **normal**, corresponde al bit `1` (ej. `xyz` → `111`).
* Si la variable aparece **complementada** (prima), corresponde al bit `0` (ej. `x'y'z'` → `000`).

**Ejemplos:**

* `m5 = xy'z` → `101`
* `m7 = xyz` → `111`

---

## Maxitérmino

| x + y + z | M0 |
|:---:|:---:|
| 0 + 0 + 0 | |

* Si la variable aparece en su forma **normal**, corresponde al bit `0` (ej. `x+y+z` → `000`).
* Si la variable aparece **complementada** (prima), corresponde al bit `1` (ej. `x'+y'+z'` → `111`).

**Ejemplos:**

* `M5 = x' + y + z'` → `101`
* `M7 = x' + y' + z'` → `111`

---

## Ejemplo — Simplificación con Álgebra Booleana

> **Condiciones:**
> * Si el segundo está de acuerdo.
> * Si el primero y el segundo están de acuerdo.

| x | y | F | Minitérmino |
|:---:|:---:|:---:|:---:|
| 0 | 0 | 0 | m0 |
| 0 | 1 | 1 | m1 |
| 1 | 0 | 0 | m2 |
| 1 | 1 | 1 | m3 |

### Implementación por álgebra booleana

* `F(x,y) = Σm(1,3)`
* `= m1 + m3`
* `= x'y + xy`
* `= y(x' + x)`
* `= y · 1`
* `= y`

### Ejemplo de diagrama de circuito

```
x----|-------|
     |       --AND----------|
y-------|----|              |
     |  |                   -----
     |  |-----------|       |
     |              --OR----|
     |-------NOT----|
```

> **ASIC (Application-Specific Integrated Circuit):** diseño especializado y particular para aquellos chips que no son fabricados de forma genérica/estándar. Ofrece ventajas de mantenimiento, mejora el *delay* (retardo) y reduce costos al simplificar y reducir la cantidad de componentes en el diseño.

---

## Ejemplo — 3 Variables

> **Condiciones:**
> * Si los 3 están de acuerdo.
> * Si el primero y el segundo están de acuerdo.
> * Si el primero y el tercero están de acuerdo.

| x | y | z | F | Minitérmino |
|:---:|:---:|:---:|:---:|:---:|
| 0 | 0 | 0 | 0 | m0 |
| 0 | 0 | 1 | 0 | m1 |
| 0 | 1 | 0 | 0 | m2 |
| 0 | 1 | 1 | 0 | m3 |
| 1 | 0 | 0 | 0 | m4 |
| 1 | 0 | 1 | 1 | m5 |
| 1 | 1 | 0 | 1 | m6 |
| 1 | 1 | 1 | 1 | m7 |

### Implementación por álgebra booleana

* `F(x,y,z) = Σm(5,6,7)`
* `= m5(101) + m6(110) + m7(111)`
* `= xy'z + xyz' + xyz`
* `= xy'z + xy(z + z')`
* `= xy'z + xy`
* `= x(y'z + y)`
* `= x(y + y')(y + z)`
* `= x(y + z)`
* `= xy + xz`

### Ejemplo de diagrama de circuito

```
x----|-----|
     |     --AND----------|
y----------|              |
     |                    ---OR---
     |-----|              |
           --AND----------|
z----------|
```

---

## Mapa de Karnaugh

> Herramienta gráfica para simplificar expresiones booleanas; representación visual del álgebra booleana.

### Mapa de 2 variables

| x\\y | 0 | 1 |
|:--:|:--:|:--:|
| **0** | x'y' → m0 | x'y → m1 |
| **1** | xy' → m2 | xy → m3 |

**Forma tradicional de posición de `x` en el mapa:**

| |
|:--:|
| x' |
| x |

**Forma tradicional de posición de `y` en el mapa:**

| y' | y |
|:--:|:---:|

Esto permite mapear utilizando dicha base.

### Ejemplo de mapeo

| x | y | F | Minitérmino |
|:---:|:---:|:---:|:---:|
| 0 | 0 | 0 | m0 |
| 0 | 1 | 0 | m1 |
| 1 | 0 | 1 | m2 |
| 1 | 1 | 1 | m3 |

| x\\y | y' | y |
|:--:|:--:|:--:|
| **x'** | | |
| **x** | 1 | 1 |

> Cuando hay un solo cambio de bit entre dos celdas, estas son **vecinas** (adyacentes).

* `00 - 01` → Son vecinos.
* `00 - 10` → Son vecinos.
* `10 - 11` → Son vecinos.
* `01 - 11` → Son vecinos.
* `00 - 11` → **No** son vecinos.

**Caso F = 1 (agrupación completa):**

| x\\y | y' | y |
|:--:|:--:|:--:|
| **x'** | 1 | 1 |
| **x** | 1 | 1 |

**Caso F = 0 (sin agrupación):**

| x\\y | y' | y |
|:--:|:--:|:--:|
| **x'** | | |
| **x** | | |

---

## Mapa de Karnaugh — 3 Variables

| x \\ yz | y'z' | y'z | yz | yz' |
|:--:|:--:|:--:|:--:|:--:|
| | **00** | **01** | **11** | **10** |
| **x' (0)** | 0 | 1 | 3 | 2 |
| **x (1)** | 4 | 5 | 7 | 6 |

Se usa el **código Gray** en el orden de las columnas (`00, 01, 11, 10`), para garantizar que exista un solo bit de cambio entre celdas vecinas:

| |
|:--:|
| 00 |
| 01 |
| 11 |
| 10 |

### Ejemplo

> `F(x,y,z) = Σ(0,3,4,7)`

| x \\ yz | y'z' | y'z | yz | yz' |
|:--:|:--:|:--:|:--:|:--:|
| | **00** | **01** | **11** | **10** |
| **x' (0)** | 1 | 0 | 1 | 0 |
| **x (1)** | 1 | 0 | 1 | 0 |

Al estar en la misma posición (columna), se alinean; de dicho mapa sale:

`F = y'z' + yz`

---

### Ejemplo

| A \\ BC | B'C' | B'C | BC | BC' |
|:--:|:--:|:--:|:--:|:--:|
| | **00** | **01** | **11** | **10** |
| **A' (0)** | 1 | 0 | 1 | 0 |
| **A (1)** | 1 | 1 | 1 | 0 |

> El minitérmino 5 se puede agrupar con otro grupo, ya sea con 4 o con 7. Al agruparlo con 4, se obtiene un inversor extra (`AB'`); en cambio, al agrupar con 7 se obtiene `AC`, sin necesidad de un inversor adicional. Se prefiere esta última agrupación por ser más simple.

`F = A'B' + BC + B'C'`

> Si hay una situación tipo XOR o XNOR, esto genera un caso especial donde no siempre se obtiene la mínima expresión posible directamente del mapa; de lo contrario, el mapa entrega el mínimo esperado.

---

### Ejemplo

| x \\ wz | w'z' | w'z | wz | wz' |
|:--:|:--:|:--:|:--:|:--:|
| | **00** | **01** | **11** | **10** |
| **x' (0)** | 1 | 1 | 1 | 1 |
| **x (1)** | 1 | 1 | 0 | 1 |

`F = z' + w' + x'`

> Al agrupar los primeros cuatro bits de la zona de `w'`, luego los bits de `z'`, y la fila de `x'`.

---

### Ejemplo

| x \\ yz | y'z' | y'z | yz | yz' |
|:--:|:--:|:--:|:--:|:--:|
| | **00** | **01** | **11** | **10** |
| **x' (0)** | 1 | 1 | 1 | 0 |
| **x (1)** | 0 | 1 | 1 | 1 |

`F = z + x'y' + xy`

---

### Ejemplo

| x \\ yz | y'z' | y'z | yz | yz' |
|:--:|:--:|:--:|:--:|:--:|
| | **00** | **01** | **11** | **10** |
| **x' (0)** | 1 | 0 | 1 | 1 |
| **x (1)** | 1 | 1 | 1 | 1 |

`F = y + y'z' + x`

---

### Ejemplo

> `f(A,B,C) = Σ(0,1,2,5,7)`

| A \\ BC | B'C' | B'C | BC | BC' |
|:--:|:--:|:--:|:--:|:--:|
| | **00** | **01** | **11** | **10** |
| **A' (0)** | 1 | 1 | 0 | 1 |
| **A (1)** | 0 | 1 | 1 | 0 |

`F = A'B' + AC + C'A'`

---

## Mapa de Karnaugh — 4 Variables

| DC \\ BA | B'A' | B'A | BA | BA' |
|:--:|:--:|:--:|:--:|:--:|
| | **00** | **01** | **11** | **10** |
| **D'C' (00)** | 0 | 1 | 5 | 2 |
| **D'C (01)** | 4 | 5 | 7 | 6 |
| **DC (10)** | 12 | 13 | 15 | 14 |
| **DC' (11)** | 8 | 9 | 11 | 10 |

### Ejemplo

| DC \\ BA | B'A' | B'A | BA | BA' |
|:--:|:--:|:--:|:--:|:--:|
| | **00** | **01** | **11** | **10** |
| **D'C' (00)** | 1 | 0 | 1 | 1 |
| **D'C (01)** | 0 | 1 | 1 | 1 |
| **DC (10)** | 0 | 1 | 1 | 0 |
| **DC' (11)** | 1 | 0 | 0 | 1 |

`F = C'A' + AC + D'B`

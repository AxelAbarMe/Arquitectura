# Diseño de Comparadores (Circuitos Combinatorios)

> Necesita una F matemática

```C++
    { A >= B; F=1
F = {
    { A < B; F=0
```

Existen 2 formas más para hacer el circuito, pero primeramente su implementación con compuertas:

| Pos m | A2 | A1 | B2 | B1 | F
|:--:|:--:|:--:|:--:|:--:|:--:|
|0|0|0|0|0|1
|1|0|0|0|1|0
|2|0|0|1|0|0
|3|0|0|1|1|0
|4|0|1|0|0|1
|5|0|1|0|1|1
|6|0|1|1|0|0
|7|0|1|1|1|0
|8|1|0|0|0|1
|9|1|0|0|1|1
|10|1|0|1|0|1
|11|1|0|1|1|0
|12|1|1|0|0|1
|13|1|1|0|1|1
|14|1|1|1|0|1
|15|1|1|1|1|1

* `Σm(0,4,5,8,9,10,12,13,14,15)`

## Mapa de Karnaugh del comparador

| A2A1 \ B2B1 | B2'B1' | B2'B1 | B2B1 | B2B1' |
|:--:|:--:|:--:|:--:|:--:|
| | **00** | **01** | **11** | **10** |
| **A2'A1' (00)** | 1 | 0 | 0 | 0 |
| **A2'A1 (01)** | 1 | 1 | 0 | 0 |
| **A2A1 (11)** | 1 | 1 | 1 | 1 |
| **A2A1' (10)** | 1 | 1 | 0 | 1 |

**Agrupaciones:**
* Fila `A2A1=11` completa (12,13,15,14) → **A2A1**
* Columna `B2B1=00` completa (0,4,12,8) → **B2'B1'**
* Bloque `{8,9,12,13}` (A2=1, B2=0, A1 y B1 libres) → **A2B2'**
* Par `{5,13}` (A1=1, B2=0, B1=1, A2 libre) → **A1B2'B1**
* Par `{10,14}` (A2=1, B2=1, B1=0, A1 libre) → **A2B2B1'**

**F(A2,A1,B2,B1) = A2A1 + B2'B1' + A2B2' + A1B2'B1 + A2B2B1'**

## Diagrama de circuito del comparador

```
A2---+----|
     |    --AND(A2A1)-------|
A1---+----|                 |
                             |
B2'--+----|                 |
     |    --AND(B2'B1')-----|
B1'--+----|                 |
                             |
A2---+----|                 |
     |    --AND(A2B2')------|----OR----> F
B2'--+----|                 |
                             |
A1---+----|                 |
     |    --AND(A1B1B2')----|
B1---+----|                 |
B2'--+----|                 |
                             |
A2---+----|                 |
     |    --AND(A2B2B1')----|
B2---+----|
B1'--+----|
```

---

## Mini términos de 3 variables (8 Mini términos)

| Mini Término | xyz | Bin |
|:--:|:--:|:--:|
| m0 | x\`y\`z\`| 000|
| m1 | x\`y\`z | 001 |
| m2 | x\`yz\` | 010 |
| m3 | x\`yz | 011 |
| m4 | xy\`z\` | 100 |
| m5 | xy\`z | 101 |
| m6 | xyz\` | 110 |
| m7 | xyz | 111 |

> Cada mini término representa **una única fila** de la tabla de verdad; por eso son la base de los decodificadores: cada salida del decodificador corresponde exactamente a un mini término.

---

## Decodificador

> Un decodificador tiene `n` entradas y **`2^n` salidas**. Solo **una salida está activa (1)** a la vez, y corresponde exactamente al mini término representado por la combinación binaria presente en las entradas.

| n (entradas) | 2^n (salidas) |
|:--:|:--:|
| 2 | 4 |
| 3 | 8 |
| 4 | 16 |

### Decodificador de 2 variables

```
          +----------------+
     A ---|0              0|---> m0 = A'B'
     B ---|1              1|---> m1 = A'B
          |                2|---> m2 = AB'
          |                3|---> m3 = AB
          +----------------+
             Decodificador 2 a 4
```

| A | B | Salida activa |
|:--:|:--:|:--:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 2 |
| 1 | 1 | 3 |

### Decodificador de 3 variables

```
          +----------------+
     x ---|0              0|---> m0 = x'y'z'
     y ---|1              1|---> m1 = x'y'z
     z ---|2              2|---> m2 = x'yz'
          |                3|---> m3 = x'yz
          |                4|---> m4 = xy'z'
          |                5|---> m5 = xy'z
          |                6|---> m6 = xyz'
          |                7|---> m7 = xyz
          +----------------+
             Decodificador 3 a 8
```

| x | y | z | Salida activa |
|:--:|:--:|:--:|:--:|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 2 |
| 0 | 1 | 1 | 3 |
| 1 | 0 | 0 | 4 |
| 1 | 0 | 1 | 5 |
| 1 | 1 | 0 | 6 |
| 1 | 1 | 1 | 7 |

### Decodificador de 4 variables

```
          +----------------+
     w ---|0              0|---> m0
     x ---|1              1|---> m1
     y ---|2               ...
     z ---|3              14|---> m14
          |               15|---> m15
          +----------------+
             Decodificador 4 a 16
```

> Sigue la misma lógica: cada una de las **16** combinaciones de `w,x,y,z` activa exactamente **una** de las 16 salidas (m0 a m15), correspondiente a su mini término.

### Decodificador aplicado a Full Adder

**Tabla de verdad del Full Adder:**

| x | y | z | S | C | Minitérmino |
|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 0 | 0 | 0 | 0 | m0 |
| 0 | 0 | 1 | 1 | 0 | m1 |
| 0 | 1 | 0 | 1 | 0 | m2 |
| 0 | 1 | 1 | 0 | 1 | m3 |
| 1 | 0 | 0 | 1 | 0 | m4 |
| 1 | 0 | 1 | 0 | 1 | m5 |
| 1 | 1 | 0 | 0 | 1 | m6 |
| 1 | 1 | 1 | 1 | 1 | m7 |

* `S = Σm(1,2,4,7)`
* `C = Σm(3,5,6,7)`

Se usa un decodificador **3 a 8** (entradas x,y,z) y se combinan las salidas correspondientes mediante compuertas OR:

```
          +----------------+
     x ---|0              0|
     y ---|1              1|---\
     z ---|2              2|----\
          |                3|-----\----OR----> S
          |                4|-----/
          |                5|----/
          |                6|
          |                7|---+----\
          +----------------+    |     \
                                 |      \--OR----> C
                  m1,m2,m4,m7 --/------/
                  m3,m5,m6,m7 -----------/
```

> El mini término `m7` se reutiliza (se conecta a ambas compuertas OR), ya que `xyz=111` produce tanto `S=1` como `C=1`.

> **ASIC (Application-Specific Integrated Circuit):** diseño especializado y particular para aquellos chips que no son fabricados de forma genérica/estándar. Ofrece ventajas de mantenimiento, mejora el *delay* (retardo) y reduce costos al simplificar y reducir la cantidad de componentes en el diseño.

> Nota: La parte de Codificador es poco usada, no se evalúa.

---

## Multiplexor

> Multiplexor siempre tiene 1 salida, pero puede tener 2^n entradas

Para el caso de 2 entradas, conecta un canal según entre GND o VCC, y ese estado es lo que determina cuál canal es el que envía la información.

```
   I0 ---\
          \  9
           >MUX>---- Y
          /  7
   I1 ---/
           |
           S
```

### Tipos de multiplexor según la cantidad de variables selectoras

| Selectores (n) | Entradas (2^n) | Nombre |
|:--:|:--:|:--:|
| 1 | 2 | MUX 2 a 1 |
| 2 | 4 | MUX 4 a 1 |
| 3 | 8 | MUX 8 a 1 |
| 4 | 16 | MUX 16 a 1 |

```
   4 a 1                         8 a 1
 I0 --|0        |            I0 --|0        |
 I1 --|1       Y|--> Y       I1 --|1       Y|--> Y
 I2 --|2        |            I2 --|2        |
 I3 --|3        |            I3 --|3        |
      |S1 S0    |            I4 --|4        |
        |  |                 I5 --|5        |
        A  B                 I6 --|6        |
                              I7 --|7        |
                                   |S2 S1 S0 |
                                     |  |  |
```

### Paso 1: Tabla de verdad

Para implementar una función con un MUX, se parte de su tabla de verdad completa, pero en vez de usar **todas** las variables como selectoras, se dejan `n-1` variables como selectoras (S2,S1,S0) y la **última variable** (residual) se deja libre: su valor en cada fila determina qué se conecta a cada canal de entrada (`0`, `1`, la variable, o su complemento).

**Ejemplo — función F(w,x,y,z), usando el comparador ya resuelto** (`x=A2, y=A1, z=B2, w=B1`):

| S2 S1 S0 (xyz) | Filas w=0 / w=1 | F | Residual asignado al canal |
|:--:|:--:|:--:|:--:|
| 000 | m0 / m1 | 1 / 0 | **w'** |
| 001 | m2 / m3 | 0 / 0 | **0 (GND)** |
| 010 | m4 / m5 | 1 / 1 | **1 (VCC)** |
| 011 | m6 / m7 | 0 / 0 | **0 (GND)** |
| 100 | m8 / m9 | 1 / 1 | **1 (VCC)** |
| 101 | m10 / m11 | 1 / 0 | **w'** |
| 110 | m12 / m13 | 1 / 1 | **1 (VCC)** |
| 111 | m14 / m15 | 1 / 1 | **1 (VCC)** |

### Paso 2: Asignación de canales del MUX 8 a 1

| Canal | S2 S1 S0 | Entrada |
|:--:|:--:|:--:|
| 0 | 000 | w' |
| 1 | 001 | 0 (GND) |
| 2 | 010 | 1 (VCC) |
| 3 | 011 | 0 (GND) |
| 4 | 100 | 1 (VCC) |
| 5 | 101 | w' |
| 6 | 110 | 1 (VCC) |
| 7 | 111 | 1 (VCC) |

### Paso 3: Implementación (8→1 MUX)

```
              8-71
              MUX
w ---|>o---\   +----------------+
            \  |0              |
Vcc --------+--|1              |
Gnd --------+--|2              |
            +--|3              |
Vcc --------+--|4              |
w' ---------+--|5             F|---> F
Vcc --------+--|6              |
Vcc --------+--|7              |
               |S2  S1  S0     |
                |   |   |
                x   y   z
```

### Optimización

En lugar de generar `w'` con un inversor por cada canal que lo requiere, se coloca **un solo inversor** a la entrada `w`, y ambas señales (`w` y `w'`) se distribuyen por cableado a los canales que las necesitan, junto con `Vcc` y `Gnd`:

```
                                  8-1 MUX
      W ----+---|>o----w'
            |         |
            |         |
Vcc --------+----+----|-----------+--------+--------+
            |    |    |           |        |        |
Gnd --------|----|----+-----------|--+     |        |
            |    |    |           |  |     |        |
            |    |    |           |  |     |        |
          +-+----+----+-----------+--+-----+--------+--+
          |0(w')  1(Gnd) 2(Vcc) 3(Gnd) 4(Vcc) 5(w') 6(Vcc) 7(Vcc)|
          |                                                      |
          |                     MUX 8 a 1                       F|---> F
          |                                                      |
          +------------------------+-------+---------------------+
                                    |       |       |
                                   S2      S1      S0
                                    |       |       |
                                    x       y       z
```

> Con este cableado, la función `F(w,x,y,z)` queda implementada con un **único inversor** (para obtener `w'`) más el MUX de 8 a 1, sin necesidad de compuertas AND/OR adicionales.

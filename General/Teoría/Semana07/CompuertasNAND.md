# Compuertas NAND

**Ley de Morgan**
> * (x · y)' = x' + y'
> * (x + y)' = x' · y'

> **Nota:** en un circuito físico real, agregar una resistencia pequeña apenas afecta el voltaje de la señal; de manera similar, colocar dos inversores (NOT) en serie no altera el resultado lógico final, ya que se cancelan entre sí, y solo introducen un pequeño retraso de propagación (letargo) en la señal.

## Conversión de compuertas AND, OR y NOT en compuertas NAND

* **Compuertas OR:** se agregan compuertas NOT dobles en cada entrada para transformarla; se toma una NOT de cada entrada junto con la OR para convertirla en NAND. La NOT sobrante de cada par se conserva y se ajusta aplicando las reglas del NOT (unir sus entradas en una NAND).
* **Compuertas NOT:** se transforman en NAND uniendo ambas entradas de la NAND con la misma variable (equivalente a invertirla).
* **Compuertas AND:** existen 2 formas de obtenerlas:
  1. Aprovechar los NOT sobrantes que quedaron de una conversión OR cercana para completar la AND.
  2. Agregar 2 NOT en la salida: una se une con la AND (formando la NAND) y la otra NOT sobrante se ajusta con las reglas del NOT.

> **Regla general de optimización:** siempre revisar qué compuertas NOT ya existen disponibles en las entradas antes de agregar nuevas, para evitar compuertas NAND innecesarias y así llegar a la opción óptima (menor cantidad de compuertas) que cumpla la función.

## Ejemplos:

**(A + B) ⋅ A' = F**
```
A ----+---[--NAND ]-----\
      \---[--NAND ]----- \______
                            |   ---[= NAND ]---+---[--NAND ]-----\
B ----+---[--NAND ]---------|--/               \---[--NAND ]----- \
      \---[--NAND ]-----/   |                                      [= NAND ]---+---[--NAND ]---> F
                            |                                     /            \---[--NAND ]---/
                            |------------------------------------/

```

**(A + B) ⋅ C = F**
```
A ----+---[--NAND ]-----\
      \---[--NAND ]----- \
                          [= NAND ]---+---[--NAND ]-----\
B ----+---[--NAND ]----- /            \---[--NAND ]----- \
      \---[--NAND ]-----/                                 [= NAND ]---+---[--NAND ]---> F
                                                          /           \---[--NAND ]---/
C -------------------------------------------------------/

```

**B(C + DE) + CD' = F**
```
D -----\
        [= NAND ]---+---[--NAND ]-----\
E -----/            \---[--NAND ]----- \
                                        [= NAND ]---+---[--NAND ]-----\
C ----+---[--NAND ]------------------- /            \---[--NAND ]----- \
      \---[--NAND ]-------------------/                                 [= NAND ]---+---[--NAND ]-----\
                                                                        /           \---[--NAND ]----- \
B ---------------------------------------------------------------------/                                \
                                                                                                         [= NAND ]---> F
C -----\                                                                                                 /
        [= NAND ]---+---[--NAND ]----+---[--NAND ]------------------------------------------------------/
D ----+---[--NAND ]/                 \---[--NAND ]-----------------------------------------------------/
      \---[--NAND ]

```

# Circuito combinacional

> La salida es una combinación de las entradas; no tiene memoria (es decir, puede cambiar sin depender del resultado anterior).
>
> El circuito combinacional más sencillo de estudiar es el sumador.

## Half Adder

* x, y son entradas
* S es Salida (Suma)
* C es Acarreo (Carry)

x|y|S|C
|:--:|:--:|:--:|:--:|
0|0|0|0
0|1|1|0
1|0|1|0
1|1|0|1

Se transforma para conocer las compuertas con un mapa K de 2 variables.

**Mapa K de 2 variables de S**

| x\y |  0 | 1 |
|:--:|:--:|:--:|
| 0 |  0 | 1
| 1 |  1 | 0

S = xy' + x'y => S = x XOR y

> El resultado se transforma en una compuerta XOR (patrón "tablero de ajedrez" en el mapa K, típico de la operación XOR, sin posibilidad de agrupar términos).

**Mapa K de 2 variables de C**

| x\y |  0 | 1 |
|:--:|:--:|:--:|
| 0 |  0 | 0
| 1 |  0 | 1

C = xy

```
x ----|---|
      |   ---XOR----S
y -|--|---|
   |  |
   |  |---|
   |      ---AND----C
   |------|
```

---

## Full Adder

* x, y, z son entradas
* S es Salida (Suma)
* C es Acarreo (Carry)

x|y|z|S|C
|:--:|:--:|:--:|:--:|:---:|
0|0|0|0|0
0|0|1|1|0
0|1|0|1|0
0|1|1|0|1
1|0|0|1|0
1|0|1|0|1
1|1|0|0|1
1|1|1|1|1

Se transforma para conocer las compuertas con un mapa K de 3 variables.

**Mapa K de 3 variables de S**

| x\yz |  00 | 01 | 11 | 10 |
|:--:|:--:|:--:|:--:|:--:|
| 0 |  0 | 1 | 0 | 1
| 1 |  1 | 0 | 1 | 0

S = x'y'z + x'yz' + xy'z' + xyz => S = x XOR y XOR z

> El resultado se transforma en dos compuertas XOR encadenadas (patrón "tablero de ajedrez" en el mapa K, sin posibilidad de agrupar términos, igual que en el Half Adder).

**Mapa K de 3 variables de C**

| x\yz |  00 | 01 | 11 | 10 |
|:--:|:--:|:--:|:--:|:--:|
| 0 |  0 | 0 | 1 | 0
| 1 |  0 | 1 | 1 | 1

> Aquí se ignora la agrupación óptima del mapa K (que daría `xy + yz + xz`) y en su lugar se toma la expresión sin combinar los términos que comparten x, para poder reutilizar el XOR ya calculado en S:

C = xy + xy'z + x'yz

C = xy + z(xy' + x'y)

C = xy + z(x XOR y)

```
x ----+---[--XOR ]-----+-------------------[--XOR ]---> S
      |                 \                     /
y ----+                  \___[ S1 ]__________/
      |                       |
      +---[--AND ]-----\      |
      |      (C1)        \    |
      |                    \  |
z ----+--------------------+--+---[--AND ]-----\
      |                                 (C2)     \
      |                                            [--OR ]---> C
      +--------------------------------------------/
```

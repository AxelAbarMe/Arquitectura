# Diseño lógico

## Primer paso

Siempre se define variables de entrada y salidas

| x y | F
|:---:|:---:
| 0 0 | 0
| 0 1 | 0
| 1 0 | 0
| 1 1 | 1

Depende de cantidad de variables, tiene 2^x. Donde x es la cantidad de variables

> Tablas de verdad permiten revisar posibles combinaciones

## Minitérmino

Minitérmino es un término booleano donde participan variables x y x`

| x y` z | m5 |
|:------:|:--:|
| 1 0  1 |

> xyz normal es 1

> x\`y\`z` prima es 0

* m5 = xy`z = 101
* m7 = xyz = 111

## Maxitérmino
| x + y + z | M0
|:---:|:---:|
| 0   0   0 |

> xyz normal es 0

> x\`y\`z` prima es 1

* M5 = x\` + y + z` = 101
* M7 = x\` + y\` + z\` = 111
---

## Ejemplo

> Condiciones:
> * Si segundo está de acuerdo
> * Si primero y segundo de acuerdo

| x y | F | Minitérmino
|:---:|:---:|:---:|
| 0 0 | 0 | m0
| 0 1 | 1 | m1
| 1 0 | 0 | m2
| 1 1 | 1 | m3

## Implementación álgebra lineal
* F(x,y) Sumatoria m(1,3)
* m1 + m3
* x\`y + xy  = y(x`+x)
* = y 1
* = y

## Ejemplo Diagrama de Circuito
```
x----|-------|
     |       --AND----------|
y-------|----|              |
     |  |                   -----
     |  |-----------|       |
     |              --OR----|
     |-------NOT----|
```

ASIC Diseño especializado particular: Para aquellos chips que no son fabricados con naturalidad

> Ofrece mantenimiento, mejora delay, costos. Reduciendo cantidad de componentes en el diseño  -> Simplificar

## Ejemplo

> Condiciones:
> * Si los 3 están de acuerdo
> * Si primero y segundo
> * Si primero y tercero

| x y | F | Minitérmino
|:---:|:---:|:---:|
| 0 0 0 | 0 | m0
| 0 0 1 | 0 | m1
| 0 1 0 | 0 | m2
| 0 1 1 | 0 | m3
| 1 0 0 | 0 | m4
| 1 0 1 | 1 | m5
| 1 1 0 | 1 | m6
| 1 1 1 | 1 | m7

## Implementación álgebra lineal
* F(x,y,z) Sumatoria m(5,6,7)
* m5 (101) + m6 (110) + m7 (111)
* xy\`z + xyz\` + xyz
* xy\`z + xy(z+z\`)
* xy\`z + xy
* x(y\`z+y)
* x(y+y\`)(y+z)
* x(y+z)
* xy+xz

## Ejemplo Diagrama de Circuito
```
x----|-----|
     |     --AND----------|
y----------|              |
     |                    ---OR---
     |-----|              |
           --AND----------|
z----------|
```

## Mapa Karnaugh

> Herramienta para simplificar
>
> Representación gráfica de algebra booleana

| x\y | 0 | 1 |
|:--:|:--:|:--:|
| 0 | x\`y\`  m0 | x\`y  m1 |
| 1 | x\`y  m2 | xy  m3  |

## Forma tradicional de posición de x en el mapa
| x`|
|:--:|
| x |

## Forma tradicional de posición de y en el mapa
| y`| y |
|:--:|:---:|

Esto permite mapear utilizando dicha base

| x y | F | Minitérmino
|:---:|:---:|:---:|
| 0 0 | 0 | m0
| 0 1 | 0 | m1
| 1 0 | 1 | m2
| 1 1 | 1 | m3

| x\y | y` | y |
|:--:|:--:|:--:|
| x` | 
| x  | 1 | 1 |

> Cuando hay cambios de bit son vecinos

* 00 - 01 Son vecinos
* 00 - 10 Son vecinos
* 10 - 11 Son vecinos
* 01 - 11 Son vecinos
* 00 - 11 No son vecinos



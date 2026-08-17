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

## F = 1
| x\y | y` | y |
|:--:|:--:|:--:|
| x` | 1 | 1 |
| x  | 1 | 1 |

## F = 0
| x\y | y` | y |
|:--:|:--:|:--:|
| x` | 
| x  |


## Mapa Karnaugh 3 variables

| \ | yz |y\`z\` | y\`z | yz | yz\`
|:--:|:--:|:--:|:--:|:--:|:--:|
|  x  | \  | 00 | 01 | 11 | 10 |
| x` | 0 |  0 | 1 | 3 | 2
| x  | 1 |  4 | 5 | 7 | 6

Usa Sistema GRAY para que exista bit de cambio

| 00 |
|:--:|
| 01 |
| 11 |
| 10 |

## Ejemplo
> F(x,y,z) = Sum(0,3,4,7)


| \ | yz |y\`z\` | y\`z | yz | yz\`
|:--:|:--:|:--:|:--:|:--:|:--:|
|  x  | \  | **00** | **01** | **11** | **10** |
| x` | **0** |  1 | 0 | 1 | 0
| x  | **1** |  1 | 0 | 1 | 0


Al estar en la misma posicion, se alinean, de dicho mapa sale:

y\`z\` + yz

---
## Ejemplo

| \ | BC |B\`C\` | B\`C | BC | BC\`
|:--:|:--:|:--:|:--:|:--:|:--:|
| A  | \  | **00** | **01** | **11** | **10** |
| A` | **0** |  1 | 0 | 1 | 0
| A  | **1** |  1 | 1 | 1 | 0

> 5 se puede agrupar con otro grupo, 4  7. Al tomar 4 obtiene un inversor extra, AB\`. En cambio, agrupar con 7 regresa AC, no existe necesidad de un inversor.

A\`B\` + BC + B\`C\`

> Si hay situación de XOR o XNOR otorga una situación especial, sino el mapa entrega el mínimo

## Ejemplo

| \ | wz |w\`z\` | w\`z | wz | wz\`
|:--:|:--:|:--:|:--:|:--:|:--:|
|  x  | \  | **00** | **01** | **11** | **10** |
| x` | **0** |  1 | 1 | 1 | 1
| x  | **1** |  1 | 1 | 0 | 1

z\` + w\` + x\`

> Al agrupar primeros cuatro bits de la zona de w\`, luego los bits de z\` y la fila de x\`

## Ejemplo

| \ | yz |y\`z\` | y\`z | yz | yz\`
|:--:|:--:|:--:|:--:|:--:|:--:|
|  x  | \  | **00** | **01** | **11** | **10** |
| x` | **0** |  1 | 1 | 1 | 0
| x  | **1** |  0 | 1 | 1 | 1

z + x\`y\` + xy

## Ejemplo

| \ | yz |y\`z\` | y\`z | yz | yz\`
|:--:|:--:|:--:|:--:|:--:|:--:|
|  x  | \  | **00** | **01** | **11** | **10** |
| x` | **0** |  1 | 0 | 1 | 1
| x  | **1** |  1 | 1 | 1 | 1

y + y\`z\` + x


## Ejemplo

> f(A,B,C) = Sum(0,1,2,5,7)

| \ | BC |B\`C\` | B\`C | BC | BC\`
|:--:|:--:|:--:|:--:|:--:|:--:|
| A  | \  | **00** | **01** | **11** | **10** |
| A` | **0** |  1 | 1 | 0 | 1
| A  | **1** |  0 | 1 | 1 | 0

A\`B\` + AC + C\`A\`


## Mapa Karnaugh 4 variables

| \ | BA |B\`A\` | B\`A | BA | BA\`
|:--:|:--:|:--:|:--:|:--:|:--:|
| DC  | \  | **00** | **01** | **11** | **10** |
| D\`C` | **00** |  0 | 1 | 5 | 2
| D\`C  | **01** |  4 | 5 | 7 | 6
| DC   | **10** |  12 | 13 | 15 | 14
| DC`  | **11** |  8 | 9 | 11 | 10

## Ejemplo

| \ | BA |B\`A\` | B\`A | BA | BA\`
|:--:|:--:|:--:|:--:|:--:|:--:|
| DC  | \  | **00** | **01** | **11** | **10** |
| D\`C` | **00** |  1 | 0 | 1 | 1
| D\`C  | **01** |  0 | 1 | 1 | 1
| DC   | **10**  |  0 | 1 | 1 | 0
| DC`  | **11**  |  1 | 0 | 0 | 1


C\`A\` + AC + D\`B


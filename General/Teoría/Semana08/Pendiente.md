# Diseño de comparadores (Circuitos combinatorios)

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


## Mini términos de 3 variables (8 Mini términos)

| Mini Término | xyz | Bin |
|:--:|:--:|:--:|
| m0 | x\`y\`z`| 000|
| m1 | x\`y`z | 001 |
| m2 | x\`yz` | 010 |
| m3 | x`yz | 011 |
| m4 | xy\`z` | 100 |
| m5 | xy`z | 101 |
| m6 | xyz` | 110 |
| m7 | xyz | 111 |

## Decodificador

Basado en el ejemplo anterior de mini términos: `Σm(0,4,5,8,9,10,12,13,14,15)`, al utilizar un decodificador de 4 variables

### Decodificador de 2 variables

### Decodificador de 3 variables

### Decodificador de 4 variables

### Decodificador aplicado a Full Adder

> Nota: La parte de Codificador es poco usada, no se evalúa.

## Multiplexor

> Multiplexor siempre tiene 1 salida, pero puede tener 2^n entradas

Para el caso de 2 entradas, conecta un canal según entre GND o VCC, y ese estado es lo que determina cuál canal es el que envía la información

### Paso 1: Tabla de verdad

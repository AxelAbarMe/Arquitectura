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
|6|0|1|1|0|1
|7|0|1|1|1|0
|8|1|0|0|0|1
|9|1|0|0|1|1
|10|1|0|1|0|1
|11|1|0|1|1|0
|12|1|1|0|0|1
|13|1|1|0|1|1
|14|1|1|1|0|1
|15|1|1|1|1|1

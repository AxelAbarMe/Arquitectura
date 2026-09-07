# Compuertas NAND

**Ley de Morgan**
> * (x * y)\` = x\`+ y\`
> * (x + y)\` = x\` * y\`

Resistencia con poco afecta el voltaje, dos inversores no afecta el resultado del voltaje y genera poco letargo.

## Conversión de compuertas AND, OR y NOT en compuertas NAND

* Compuertas OR se les agrega en las entradas compuertas NOT dobles para transformarla, se agarra una NOT de cada entrada junto con el OR para volverla NAND, el sobrante NOT se deja y se modifica con las reglas del NOT.
* Compuertas NOT se transforman en NAND uniendo ambas entradas de una misma variable para transformarlo en NAND.
* Compuertas AND tiene 2 formas, se aprovecha de los NOT abandonados del OR para volverse AND o agrega en la salida 2 NOT y une 1 con AND y el otro sobrante NOT se deja y se modifica con las reglas del NOT.

> Revisar que se usa en las entradas para evitar agregar compuertas NAND innecesarias, buscar opción óptima de menos compuertas que cumpla la función.

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

> Salida es combinación de las entradas, no tiene memoria (Ósea puede cambiar sin depender del resultado anterior)
>
> Circuito mas sencillo es el sumador

## Half Adder

# Tarea 2
## Sistemas Numéricos y Códigos Binarios

## Parte A

### 1. Ejecute las siguientes operaciones y conversiones:

* a- 11101)₂ x 101011)₂ = xxxx)₂
* b- 101)₂ x 10101)₂ = xxxx)₂

* c- Complete las siguientes tablas:

| BCD | 1000 | 0101 | 0110 | ……………. | ……………. |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Gray | ……………. | ……………. | ……………. | 1001 | 1110 |

| Bin | 10111100 | 0101000 | 1110110 | ……………. | ……………. |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Gray | ……………. | ……………. | ……………. | 10010000 | 1111000000 |

| Gray | 10111100001 | 0101000111 | 111011011 | ……………. | ……………. |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Bin | ……………. | ……………. | ……………. | 1000110000 | 0111000000 |

* d- Suma de BCD:

```
   479+        509+        831+
   999         234         586
----------  ----------  ----------
```

---

### 2. Realice las siguientes operaciones utilizando la técnica de complemento a dos:

* a) 1111 - 11)₂ =
* b) 111 - 00001)₂ =
* c) 100000 - 110)₂ =
* d) 0001 - 10)₂ =
* e) 0101010 - 111111111)₂ =
* f) 00000011 - 1111000)₂ =
* g) 111111 - 10101)₂ =

---

### 3.

* a) ¿Cuál es la representación del código ASCII - A (letra A) transmitido con paridad IMPAR? (Utilizar la tabla de los códigos ASCII y encontrar el código de letra A)
* b) Decodifique el código binario de ASCII siguiente:

```
1001010  1100001  1101110  1100101  0100000  1000100  1101111  1100101
```

> Tip: Cada 7 bits representa una letra de ASCII

---

### 4. Llenar los campos vacíos con la información (P) que se solicita de acuerdo con el modo de transmisión:

| Paridad | P | 2⁶ | 2⁵ | 2⁴ | 2³ | 2² | 2¹ | 2⁰ | Carácter |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Par | ☐ | 1 | 1 | 0 | 0 | 0 | 0 | 1 | a |
| Impar | ☐ | 1 | 1 | 0 | 0 | 0 | 1 | 0 | b |
| Par | ☐ | 1 | 1 | 0 | 0 | 0 | 1 | 1 | c |
| Impar | ☐ | 1 | 1 | 1 | 1 | 0 | 1 | 0 | z |
| Par | ☐ | 1 | 0 | 0 | 0 | 0 | 0 | 1 | A |

---

## Parte B: Aplicación de Python en Arquitectura de Computadoras

1. Desarrolle un algoritmo en Python que **multiplique dos números binarios** mostrando el proceso paso a paso.
2. Desarrolle un algoritmo que convierta un número **binario básico a código Gray**.
3. Desarrolle un algoritmo que convierta un número en **código Gray a binario**.
4. Desarrolle un algoritmo que convierta **BCD a Gray y viceversa** (usando binario como paso intermedio).
5. Desarrolle un algoritmo que **sume dos números en BCD**, aplicando la corrección correspondiente cuando el resultado supere 9 o genere acarreo.
6. Desarrolle un algoritmo que **reste dos números binarios** utilizando la técnica de complemento a dos.
7. Desarrolle un algoritmo que codifique una letra en **ASCII de 7 bits agregando el bit de paridad** (par o impar) según se indique.

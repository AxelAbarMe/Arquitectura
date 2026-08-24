# Tarea 2
## Sistemas Numéricos y Códigos Binarios

## Parte A

### 1. Ejecute las siguientes operaciones y conversiones:

* a- 11101)₂ x 101011)₂ = xxxx)₂

```C++
     101011
      11101
     ------
     101011
    000000
   101011
  101011
+101011
-----------
10011011111
```

> Respuesta `10011011111₂`

* b- 101)₂ x 10101)₂ = xxxx)₂

```C++
     10101
       101
     -----
     10101
    00000
 + 10101
-----------
   1101001
```

> Respuesta `1101001₂`

* c- Complete las siguientes tablas:

| BCD | 1000 | 0101 | 0110 | ……………. | ……………. |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Gray | ……………. | ……………. | ……………. | 1001 | 1110 |

1. `1000`

```
Número 1000₂

Proceso:

1 + 0 + 0 + 0
|  \   \   \
1   1   0   0

Primer par: 1 XOR 0 = 1
Segundo par: 0 XOR 0 = 0
Tercer par: 0 XOR 0 = 0

Resultado final:
1000₂
|
v
1100 (G)
```

2. `0101`

```
Número 0101₂

Proceso:

0 + 1 + 0 + 1
|  \   \   \
0   1   1   1

Primer par: 0 XOR 1 = 1
Segundo par: 1 XOR 0 = 1
Tercer par: 0 XOR 1 = 1

Resultado final:
0101₂
|
v
0111 (G)
```

3. `0110`

```
Número 0110₂

Proceso:

0 + 1 + 1 + 0
|  \   \   \
0   1   0   1

Primer par: 0 XOR 1 = 1
Segundo par: 1 XOR 1 = 0
Tercer par: 0 XOR 1 = 1

Resultado final:
0110₂
|
v
0101 (G)
```

4. `1001`

```
Número 1001₂

Proceso:

1  0  0  1
|+/|+/|+/|
1  1  1  0

Primer par: 1 XOR 0 = 1
Segundo par: 1 XOR 0 = 1
Tercer par: 1 XOR 1 = 0

Al 1110 > 1001 se debe de ajustar a formato BCD
1110 = 14

1 -> 0001
4 -> 0100

Resultado final:
1001 (G)
|
v
0001 0100₂
```

5. `1110`

```
Número 1110₂

Proceso:

1  1  1  0
|+/|+/|+/|
1  0  1  1

Primer par: 1 XOR 1 = 0
Segundo par: 0 XOR 1 = 1
Tercer par: 1 XOR 0 = 1

Al 1011 > 1001 se debe de ajustar a formato BCD
1010 = 11

1 -> 0001
1 -> 0001

Resultado final:
1001 (G)
|
v
0001 0001₂
```


> Respuesta:

| BCD | 1000 | 0101 | 0110 | `0001 0100` | `0001 0001` |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Gray | `1100` | `0111` | `0101` | 1001 | 1110 |
---

| Bin | 10111100 | 0101000 | 1110110 | ……………. | ……………. |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Gray | ……………. | ……………. | ……………. | 10010000 | 1111000000 |

1. `10111100`

```
Número 10111100₂

Proceso:

1 + 0 + 1 + 1 + 1 + 1 + 0 + 0
|  \   \   \   \   \   \   \
1   1   1   0   0   0   1   0

Primer par: 1 XOR 0 = 1
Segundo par: 0 XOR 1 = 1
Tercer par: 1 XOR 1 = 0
Cuarto par: 1 XOR 1 = 0
Quinto par: 1 XOR 1 = 0
Sexto par: 1 XOR 0 = 1
Septimo par: 0 XOR 0 = 0

Resultado final:
10111100₂
|
v
11100010 (G)
```

2. `0101000`

```
Número 0101000₂

Proceso:

0 + 1 + 0 + 1 + 0 + 0 + 0
|  \   \   \   \   \   \ 
0   1   1   1   1   0   0 

Primer par: 0 XOR 1 = 1
Segundo par: 1 XOR 0 = 1
Tercer par: 0 XOR 1 = 1
Cuarto par: 1 XOR 0 = 1
Quinto par: 0 XOR 0 = 0
Sexto par: 0 XOR 0 = 0

Resultado final:
0101000₂
|
v
0111100 (G)
```

3. `1110110`

```
Número 1110110₂

Proceso:

1 + 1 + 1 + 0 + 1 + 1 + 0
|  \   \   \   \   \   \ 
1   0   0   1   1   0   1 

Primer par: 1 XOR 1 = 0
Segundo par: 1 XOR 1 = 0
Tercer par: 1 XOR 0 = 1
Cuarto par: 0 XOR 1 = 1
Quinto par: 1 XOR 1 = 0
Sexto par: 1 XOR 0 = 1

Resultado final:
1110110₂
|
v
1001101 (G)
```

4. `10010000`

```
Número 10010000₂

Proceso:

1  0  0  1  0  0  0  0
|+/|+/|+/|+/|+/|+/|+/|
1  1  1  0  0  0  0  0

Primer par: 1 XOR 0 = 1
Segundo par: 1 XOR 0 = 1
Tercer par: 1 XOR 0 = 0
Cuarto par: 0 XOR 0 = 0
Quinto par: 0 XOR 0 = 0
Sexto par: 0 XOR 0 = 0
Septimo par: 0 XOR 0 = 0

Resultado final:
10010000 (G)
|
v
11100000₂
```

5. `1111000000`

```
Número 1111000000₂

Proceso:

1  1  1  1  0  0  0  0  0  0
|+/|+/|+/|+/|+/|+/|+/|+/|+/|
1  0  1  0  0  0  0  0  0  0

Primer par: 1 XOR 1 = 0
Segundo par: 0 XOR 1 = 1
Tercer par: 1 XOR 1 = 0
Cuarto par: 0 XOR 0 = 0
Quinto par: 0 XOR 0 = 0
Sexto par: 0 XOR 0 = 0
Septimo par: 0 XOR 0 = 0
Octavo par: 0 XOR 0 = 0
Noveno par: 0 XOR 0 = 0

Resultado final:
1111000000 (G)
|
v
1010000000₂
```


> Respuesta:

| Bin | 10111100 | 0101000 | 1110110 | `11100000₂` | `1010000000₂` |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Gray | `11100010` | `0111100` | `1001101` | 10010000 | 1111000000 |
---

| Gray | 10111100001 | 0101000111 | 111011011 | ……………. | ……………. |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Bin | ……………. | ……………. | ……………. | 1000110000 | 0111000000 |

1. `10111100001`

```
Número 1111000000₂

Proceso:

1  0  1  1  1  1  0  0  0  0  1
|+/|+/|+/|+/|+/|+/|+/|+/|+/|+/|
1  1  0  1  0  1  1  1  1  1  0

Primer par: 1 XOR 0 = 1
Segundo par: 1 XOR 1 = 0
Tercer par: 0 XOR 1 = 1
Cuarto par: 1 XOR 1 = 0
Quinto par: 0 XOR 1 = 1
Sexto par: 1 XOR 0 = 1
Septimo par: 1 XOR 0 = 1
Octavo par: 1 XOR 0 = 1
Noveno par: 1 XOR 0 = 1
Decimo par: 1 XOR 1 = 0

Resultado final:
10111100001 (G)
|
v
11010111110₂
```

2. `0101000111`

```
Número 0101000111₂

Proceso:

0  1  0  1  0  0  0  1  1  1
|+/|+/|+/|+/|+/|+/|+/|+/|+/|
0  1  1  0  0  0  0  1  0  1

Primer par: 0 XOR 1 = 1
Segundo par: 1 XOR 0 = 1
Tercer par: 1 XOR 1 = 0
Cuarto par: 0 XOR 0 = 0
Quinto par: 0 XOR 0 = 0
Sexto par: 0 XOR 0 = 0
Septimo par: 0 XOR 1 = 1
Octavo par: 1 XOR 1 = 0
Noveno par: 0 XOR 1 = 1

Resultado final:
0101000111 (G)
|
v
0110000101₂
```

3. `111011011`

```
Número 0101000111₂

Proceso:

1  1  1  0  1  1  0  1  1
|+/|+/|+/|+/|+/|+/|+/|+/|
1  0  1  1  0  1  1  0  1

Primer par: 1 XOR 1 = 0
Segundo par: 0 XOR 1 = 1
Tercer par: 1 XOR 0 = 1
Cuarto par: 1 XOR 1 = 0
Quinto par: 0 XOR 1 = 1
Sexto par: 1 XOR 0 = 1
Septimo par: 1 XOR 1 = 0
Octavo par: 0 XOR 1 = 1

Resultado final:
111011011 (G)
|
v
101101101₂
```

4. `1000110000`

```
Número 1000110000₂

Proceso:

1 + 0 + 0 + 0 + 1 + 1 + 0 + 0 + 0 + 0
|  \   \   \   \   \   \   \   \   \ 
1   1   0   0   1   0   1   0   0   0

Primer par: 1 XOR 0 = 1
Segundo par: 0 XOR 0 = 0
Tercer par: 0 XOR 0 = 0
Cuarto par: 0 XOR 1 = 1
Quinto par: 1 XOR 1 = 0
Sexto par: 1 XOR 0 = 1
Septimo par: 0 XOR 0 = 0
Octavo par: 0 XOR 0 = 0
Noveno par: 0 XOR 0 = 0

Resultado final:
1000110000₂
|
v
1100101000 (G)
```

5. `0111000000`

```
Número 0111000000₂

Proceso:

0 + 1 + 1 + 1 + 0 + 0 + 0 + 0 + 0 + 0
|  \   \   \   \   \   \   \   \   \ 
0   1   0   0   1   0   0   0   0   0

Primer par: 0 XOR 1 = 1
Segundo par: 1 XOR 1 = 0
Tercer par: 1 XOR 1 = 0
Cuarto par: 1 XOR 0 = 1
Quinto par: 0 XOR 0 = 0
Sexto par: 0 XOR 0 = 0
Septimo par: 0 XOR 0 = 0
Octavo par: 0 XOR 0 = 0
Noveno par: 0 XOR 0 = 0

Resultado final:
0111000000₂
|
v
0100100000 (G)
```

> Respuesta:

| Gray | 10111100001 | 0101000111 | 111011011 | `1100101000` | `0100100000` |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Bin | `11010111110₂` | `0110000101₂` | `101101101₂` | 1000110000 | 0111000000 |

* d- Suma de BCD:

```
   479+        509+        831+
   999         234         586
----------  ----------  ----------
```

1. `479+999`

```C++
 479
+999
-------
1478


 o bien


 0100 0111 1001
+1001 1001 1001
------------------


Equivalencias:


  0100 0111 1001
+ 1001 1001 1001
------------------
 1101 10000 10010
+            0110
          --------
          [1]1000   Y se genera acarreo
      10001
  +    0110
    -------
    [1]0111    Y se genera acarreo
1110
0110
----
[1]0100

1  4  7  8

Resultado->  0001 0100 0111 1000  -> 1478
```

2. `509+234`

```C++
 509
+234
-------
 743

 o bien

 0101 0000 1001
+0010 0011 0100
------------------


Equivalencias:


  0101 0000 1001
+ 0010 0011 0100
------------------
  0111 0011 1101
+           0110
          --------
          [1]0011   Y se genera acarreo
       0011
  +       1
    -------
       0100
0111

  7    4    3

Resultado->  0111 0100 0011  -> 743
```

3. `831+589`

```C++
 831
+586
-------
1417

 o bien

  1000 0011 0001
+ 0101 1000 0110
------------------


Equivalencias:


  1000 0011 0001
+ 0101 1000 0110
------------------
  1101 1011 0111
  +    0110
    -------
    [1]0001
 1110
+0110
-----
[1]0100

1 4 1 7

Resultado->  0001 0100 0001 0111  -> 1417
```

|Suma BCD     |479+999|509+234|831+586
|:--:|:--:|:--:|:--:|
|Valor BCD    |`0001 0100 0111 1000`|`0111 0100 0011`|`0001 0100 0001 0111`
|Valor Decimal|1478|743|1417

---

### 2. Realice las siguientes operaciones utilizando la técnica de complemento a dos:

* a) `1111 - 11)₂` 

1. Unificar bases: 1111₂ - 0011₂
2. Complemento a 2 de 0011₂: **1101₂**
3. Se suma;
```
  1111
+ 1101
------
[1]1100     // Con Acarreo
```
4. Se elimina el acarreo final, completando el resultado: 1100₂

### **Comprobación:**
* Decimal: $15 - 3 = 12_{10}$
* Binario: $12_{10} = \mathbf{1100_2}$

---

* b) `111 - 00001)₂` 

1. Unificar bases: 00111₂ - 00001₂
2. Complemento a 2 de 00001₂: **11111₂**
3. Se suma;
```
 00111
+11111
------
[1]00110     // Con Acarreo
```
4. Se elimina el acarreo final, completando el resultado: 110₂

### **Comprobación:**
* Decimal: $7 - 1 = 6_{10}$
* Binario: $6_{10} = \mathbf{110_2}$

---

* c) `100000 - 110)₂` 

1. Unificar bases: 100000₂ - 000110₂
2. Complemento a 2 de 000110₂: **111010₂**
3. Se suma;
```
  100000
+ 111010
--------
[1]011010     // Con Acarreo
```
4. Se elimina el acarreo final, completando el resultado: 11010₂

### **Comprobación:**
* Decimal: $32 - 6 = 26_{10}$
* Binario: $26_{10} = \mathbf{11010_2}$

---

* d) `0001 - 10)₂` 

1. Unificar bases: 0001₂ - 0010₂
2. Complemento a 2 de 0010₂: **1110₂**
3. Se suma;
```
  0001
+ 1110
------
  1111     
```
4. Como no hubo acarreo. Se vuelve a complementar: complemento a 2 de 1111₂ = 0000₂ + 1₂ = 0001₂ Resultado final: -0001₂

### **Comprobación:**
* Decimal: $1 - 2 = -1_{10}$
* Binario: $-1_{10} = \mathbf{-0001_2}$

---

* e) `0101010 - 111111111)₂` 

1. Unificar bases: 000101010₂ - 111111111₂
2. Complemento a 2 de 111111111₂: **000000001₂**
3. Se suma;
```
  000101010
+ 000000001
------
  000101011
```
4. Como no hubo acarreo. Se vuelve a complementar: complemento a 2 de 000101011₂ = 111010100₂ + 1₂ = 111010101₂ Resultado final: -111010101₂

### **Comprobación:**
* Decimal: $42 - 511 = -469_{10}$
* Binario: $-469_{10} = \mathbf{-111010101_2}$

---

* f) `00000011 - 1111000)₂` 

1. Unificar bases: 00000011₂ - 01111000₂
2. Complemento a 2 de 01111000₂: **10001000₂**
3. Se suma;
```
  00000011
+ 10001000
-----------
  10001011     
```
4. Como no hubo acarreo. Se vuelve a complementar: complemento a 2 de 10001011₂ = 01110100₂ + 1₂ = 01110101₂ Resultado final: -01110101₂

### **Comprobación:**
* Decimal: $3 - 120 = -117_{10}$
* Binario: $-117_{10} = \mathbf{01110101_2}$

---

* g) `111111 - 10101)₂` 

1. Unificar bases: 111111₂ - 010101₂
2. Complemento a 2 de 010101₂: **101011₂**
3. Se suma;
```
  111111
+ 101011
--------
[1]101010      // Con Acarreo
```
4. Se elimina el acarreo final, completando el resultado: 101010₂

### **Comprobación:**
* Decimal: $63 - 21 = 42_{10}$
* Binario: $42_{10} = \mathbf{101010_2}$

|Enunciado|Resultados|
|:--:|:--:|
|1111 - 11)₂           |`1100`
|111 - 00001)₂         |`110`
|100000 - 110)₂        |`11010`
|0001 - 10)₂           |`-0001`
|0101010 - 111111111)₂ |`-111010101`
|00000011 - 1111000)₂  |`-01110101`
|111111 - 10101)₂      |`101010`

---

### 3.

* a) ¿Cuál es la representación del código ASCII - A (letra A) transmitido con paridad IMPAR? (Utilizar la tabla de los códigos ASCII y encontrar el código de letra A)

|Paridad| MSB | b7 | b6 | b5 | b4 | b3 | b2 | b1 |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| Impar | [ 1 ] | 1 | 0 | 0 | 0 | 0 | 0 | 1

* b) Decodifique el código binario de ASCII siguiente:

```
1001010  1100001  1101110  1100101  0100000  1000100  1101111  1100101
```

* `Solución`

```C++
b7b6b5  b4b3b2b1
 1 0 0  1 0 1 0 -> J
 1 1 0  0 0 0 1 -> a
 1 1 0  1 1 1 0 -> n
 1 1 0  0 1 0 1 -> e
 0 1 0  0 0 0 0 -> SP  //Espacio
 1 0 0  0 1 0 0 -> D
 1 1 0  1 1 1 1 -> o
 1 1 0  0 1 0 1 -> e
```

|1001010|  1100001|  1101110|  1100101|  0100000|  1000100|  1101111|  1100101|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|J|a|n|e| |D|o|e|

> Tip: Cada 7 bits representa una letra de ASCII

---

### 4. Llenar los campos vacíos con la información (P) que se solicita de acuerdo con el modo de transmisión:

| Paridad | P | 2⁶ | 2⁵ | 2⁴ | 2³ | 2² | 2¹ | 2⁰ | Carácter |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Par | [1] | 1 | 1 | 0 | 0 | 0 | 0 | 1 | a |
| Impar | [0] | 1 | 1 | 0 | 0 | 0 | 1 | 0 | b |
| Par | [0] | 1 | 1 | 0 | 0 | 0 | 1 | 1 | c |
| Impar | [0] | 1 | 1 | 1 | 1 | 0 | 1 | 0 | z |
| Par | [0] | 1 | 0 | 0 | 0 | 0 | 0 | 1 | A |

---

## Parte B: Aplicación de Python en Arquitectura de Computadoras

1. Desarrolle un algoritmo en Python que **multiplique dos números binarios** mostrando el proceso paso a paso.

```python
import sys

def producto_binario(B1,B2):
    try:
        int(B1,2)
        int(B2,2)
    except ValueError:
        print(f"Valores no binarios")
        return False
        
    b1 = str(B1)
    b2 = str(B2)

    prod_parcial = []

    for i, bit in enumerate(reversed(b2)):
        if bit == '1':
            parcial = b1 + '0' * i
        else:
            parcial = '0' * (len(b1) + i)
        prod_parcial.append(parcial)
        print(f"Bit: {bit}, Pos: {i}, Producto_ Parcial: {parcial}")
    ancho_sumando = len(prod_parcial[-1])
    for s in prod_parcial:
        print(f"Sumandos: {s.rjust(ancho_sumando)}")
    return bin(sum(int(p,2) for p in prod_parcial))[2:]

if __name__ == "__main__":
    if len(sys.argv) > 2:
        print(f"El resultado fue: {producto_binario(sys.argv[1],sys.argv[2])}")
   # python <nombreArchivo.py> <Bin1> <Bin2>
   # python ej01.py 1111 111
```

2. Desarrolle un algoritmo que convierta un número **binario básico a código Gray**.

```python
import sys

def bin_gray(B1):
    try:
        int(B1,2)
    except ValueError:
        print(f"Valores no binarios")
        return False
    b1 = str(B1)
    gray = b1[0]
    print(f"Bit 0 (MSB): {b1[0]} -> Gray: {gray}")
    for i in range(1, len(b1)):
        anterior = b1[i-1]
        actual = b1[i]
        digito = '0' if anterior==actual else '1'
        gray+=digito
        print(f"Bit {i}: B[{i-1}]={anterior} XOR B[{i}]={actual} -> {digito}")

    return gray
                
if __name__ == "__main__":
    if len(sys.argv) > 1:
        print(f"El resultado fue: {bin_gray(sys.argv[1])}")
   # python <nombreArchivo.py> <Binario>
   # python ej02.py 100111
```

3. Desarrolle un algoritmo que convierta un número en **código Gray a binario**.

```python
import sys

def gray_bin(Gray):
    try:
        int(Gray,2)
    except ValueError:
        print(f"Valores no binarios")
        return False
    
    gray = str(Gray)
    b1 = gray[0]
    print(f"Bit 0 (MSB): {gray[0]} -> Binario: {b1}")

    anterior = gray[0]
    for i in range(1, len(gray)):
        actual = gray[i]
        digito = '0' if anterior==actual else '1'
        print(f"Bit {i}: B[{i-1}]={anterior} XOR G[{i}]={actual} -> {digito}")
        b1 += digito
        anterior = digito

    return b1
                
if __name__ == "__main__":
    if len(sys.argv) > 1:
        print(f"El resultado fue: {gray_bin(sys.argv[1])}")
   # python <nombreArchivo.py> <GrayBin>
   # python ej03.py 110100
```

4. Desarrolle un algoritmo que convierta **BCD a Gray y viceversa** (usando binario como paso intermedio).

```python
import sys

def bin_gray(B1):
    try:
        int(B1,2)
    except ValueError:
        print(f"Valores no binarios")
        return False
    b1 = str(B1)
    gray = b1[0]
    print(f"Bit 0 (MSB): {b1[0]} -> Gray: {gray}")
    for i in range(1, len(b1)):
        anterior = b1[i-1]
        actual = b1[i]
        digito = '0' if anterior==actual else '1'
        gray+=digito
        print(f"Bit {i}: B[{i-1}]={anterior} XOR B[{i}]={actual} -> {digito}")

    return gray

def gray_bin(Gray):
    try:
        int(Gray,2)
    except ValueError:
        print(f"Valores no binarios")
        return False
    
    gray = str(Gray)
    b1 = gray[0]
    print(f"Bit 0 (MSB): {gray[0]} -> Binario: {b1}")

    anterior = gray[0]
    for i in range(1, len(gray)):
        actual = gray[i]
        digito = '0' if anterior==actual else '1'
        print(f"Bit {i}: B[{i-1}]={anterior} XOR G[{i}]={actual} -> {digito}")
        b1 += digito
        anterior = digito

    return b1

def bcd_dec(BCD):
    if len(BCD) % 4 != 0:
        raise ValueError("Cadena BCD inválida")

    dec=""
    for i in range(0, len(BCD), 4):
        bits = BCD[i:i+4]
        digito = int(bits,2)
        if digito > 9:
            raise ValueError("BCD inválida")
        print(f"Bits BCD: {bits} -> Dígito decimal: {digito}")
        dec += str(digito)
    return dec

    
def dec_bcd(decimal):
    bcd=""
    for dec in decimal:
        bits = bin(int(dec))[2:].zfill(4)
        print(f"Dígito decimal: {dec} -> Bits BCD: {bits}")
        bcd += bits
    return bcd

def bcd_gray(BCD):

    try:
        for i in range(0, len(BCD), 4):
            int(BCD[i:i+4],2)
    except ValueError:
        print(f"Valores no binarios")
        return False

    print("--- Paso 1: BCD -> Decimal ---")
    decimal = bcd_dec(BCD)

    print("--- Paso 2: Decimal -> Binario ---")
    binario = bin(int(decimal))[2:]
    print(f"Decimal {decimal} -> Binario {binario}")

    print("--- Paso 3: Binario -> Gray ---")
    gray = bin_gray(binario)

    return gray

def gray_bcd(Gray):
    try:
        int(Gray, 2)
    except ValueError:
        print("Valores no binarios")
        return False

    print("--- Paso 1: Gray -> Binario ---")
    binario = gray_bin(Gray)

    print("--- Paso 2: Binario -> Decimal ---")
    decimal = str(int(binario, 2))
    print(f"Binario {binario} -> Decimal {decimal}")

    print("--- Paso 3: Decimal -> BCD ---")
    bcd = dec_bcd(decimal)

    return bcd

if __name__ == "__main__":
    if len(sys.argv) > 2:
        modo = sys.argv[1]   # "bcd2gray" o "gray2bcd"
        valor = sys.argv[2]
        if modo == "bcd2gray":
            print(f"\nEl resultado fue: {bcd_gray(valor)}")
        elif modo == "gray2bcd":
            print(f"\nEl resultado fue: {gray_bcd(valor)}")
        else:
            print("Modo inválido. Usa 'bcd2gray' o 'gray2bcd'.")

   # python <nombreArchivo.py> <Modo> <Binario>
   # python ej04.py bcd2gray 01000111
   # python ej04.py gray2bcd 111000
```

5. Desarrolle un algoritmo que **sume dos números en BCD**, aplicando la corrección correspondiente cuando el resultado supere 9 o genere acarreo.

```python
import sys

def bcd_dec(BCD):
    if len(BCD) % 4 != 0:
        raise ValueError("Cadena BCD inválida")

    dec=""
    for i in range(0, len(BCD), 4):
        bits = BCD[i:i+4]
        digito = int(bits,2)
        if digito > 9:
            raise ValueError("BCD inválida")
        print(f"Bits BCD: {bits} -> Dígito decimal: {digito}")
        dec += str(digito)
    return dec

def suma_bcd(BCD1,BCD2):
    try:
        d1 = bcd_dec(BCD1)
        d2 = bcd_dec(BCD2)
    except ValueError as e:
        print(f"Valores no válidos: {e}")
        return False
    n = max(len(d1),len(d2))
    bits1 = [BCD1[i:i+4] for i in range(0, len(BCD1), 4)]
    bits2 = [BCD2[i:i+4] for i in range(0, len(BCD2), 4)]

    while len(bits1) < n:
        bits1.insert(0,'0000')
    while len(bits2) < n:
        bits2.insert(0,'0000')

    resultado = []
    acarreo = 0

    for i in range(n-1,-1,-1):
        bit1 = int(bits1[i],2)
        bit2 = int(bits2[i],2)
        sum_bin = bit1 + bit2 + acarreo
        print(f"Dígito {n-1-i}: {bits1[i]}({bit1})+{bits2[i]}({bit2}) + acarreo({acarreo}) = {sum_bin}")

        if sum_bin > 9:
            sum_new = sum_bin + 6
            acarreo = 1
            print(f" {sum_bin} > 9. Se corrige con 0110(6) -> {sum_new}, acarreo = 1")
        else:
            sum_new = sum_bin
            acarreo = 0

        bit_resultado = bin(sum_new % 16)[2:].zfill(4)
        resultado.insert(0,bit_resultado)
        print(f"Resultado Bits -> {bit_resultado}")

    if acarreo:
        resultado.insert(0,'0001')
        print(f"Acarreo: Se agrega 0001")
    resultado = ''.join(resultado)

    return resultado

if __name__ == "__main__":
    if len(sys.argv) > 2:
        print(f"\nEl resultado fue: {suma_bcd(sys.argv[1], sys.argv[2])}")

   # python <nombreArchivo.py> <BCD1> <BCD2>
   # python ej05.py 1000 0111
```

6. Desarrolle un algoritmo que **reste dos números binarios** utilizando la técnica de complemento a dos.

```python
import sys

def sum_bin(a, b):
    n = len(a)
    resultado = ""
    acarreo = 0
    for i in range(n - 1, -1, -1):
        bit_suma = int(a[i]) + int(b[i]) + acarreo
        resultado = str(bit_suma % 2) + resultado
        acarreo = bit_suma // 2
    return resultado, acarreo


def comp1(B):
    return ''.join('1' if bit == '0' else '0' for bit in B)


def comp2(B):
    c1 = comp1(B)
    c2, _ = sum_bin(c1, '0' * (len(c1) - 1) + '1')
    print(f"  Complemento a 1: {c1}")
    print(f"  + 1:             {'0' * (len(c1) - 1) + '1'}")
    print(f"  Complemento a 2: {c2}")
    return c2


def resta_binaria(B1, B2):
    try:
        int(B1, 2)
        int(B2, 2)
    except ValueError:
        print("Valores no binarios")
        return False

    n = max(len(B1), len(B2)) + 1
    a = B1.zfill(n)
    b = B2.zfill(n)

    print(f"A = {a}")
    print(f"B = {b}")
    print(f"\nPaso 1: Complemento a 2 de B")
    comp_b = comp2(b)

    print(f"\nPaso 2: Sumar A con complemento a 2 de B")
    print(f"  {a}")
    print(f"+ {comp_b}")
    suma, acarreo = sum_bin(a, comp_b)
    print(f"  {'-' * n}")
    print(f"  {suma}  (acarreo de salida: {acarreo})")

    if acarreo == 1:
        print(f"\nAcarreo de salida = 1 -> se descarta -> resultado positivo")
        resultado = suma.lstrip('0') or '0'
        return resultado
    else:
        print(f"\nAcarreo de salida = 0 -> resultado negativo, se complementa de nuevo")
        new_resultado = comp2(suma)
        new_resultado = new_resultado.lstrip('0') or '0'
        return f"-{new_resultado}"


if __name__ == "__main__":
    if len(sys.argv) > 2:
        print(f"\nEl resultado fue: {resta_binaria(sys.argv[1], sys.argv[2])}")

   # python <nombreArchivo.py> <Bin1> <Bin2>
   # python ej06.py 100 10
```

7. Desarrolle un algoritmo que codifique una letra en **ASCII de 7 bits agregando el bit de paridad** (par o impar) según se indique.

```python
import sys

def calcular_bit_paridad(codigo7, tipo):
    cantUnos = codigo7.count('1')
    print(f"7 bits: {codigo7} (cantidad de unos: {cantUnos})")

    if tipo == 'par':
        bit_paridad = '0' if cantUnos % 2 == 0 else '1'
    elif tipo == 'impar':
        bit_paridad = '1' if cantUnos % 2 == 0 else '0'
    else:
        raise ValueError("El tipo de paridad debe ser 'par' o 'impar'")

    print(f"Paridad: {tipo} -> bit de paridad: {bit_paridad}")
    return bit_paridad


def codificar_ascii_paridad(caracter, tipo='par'):
    if len(caracter) != 1:
        print("Solo 1 caracter permitido")
        return False

    valor_ascii = ord(caracter)
    if valor_ascii > 127:
        print("Caracter fuera de rango (0-127)")
        return False

    codigo7 = bin(valor_ascii)[2:].zfill(7)
    print(f"Caracter: '{caracter}' -> ASCII decimal: {valor_ascii} -> Binario (7 bits): {codigo7}")

    bit_paridad = calcular_bit_paridad(codigo7, tipo)

    codigo_final = bit_paridad + codigo7
    print(f"ASCII 8 bits, con paridad ({tipo}): {codigo_final}")

    return codigo_final


if __name__ == "__main__":
    if len(sys.argv) > 2:
        caracter = sys.argv[1]
        tipo = sys.argv[2]
        print(f"\nEl resultado fue: {codificar_ascii_paridad(caracter, tipo)}")
   # python <nombreArchivo.py> <caracter> <tipo>
   # python ej07.py A par
   # python ej07.py A impar
```

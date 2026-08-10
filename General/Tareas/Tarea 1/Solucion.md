# Tarea 1

## Tarea 1A

### 1.- Convertir del sistema Decimal al sistema Binario y Hexadecimal las siguientes cantidades: (2bits de resolución)

> Parte entera

Div |649/2 | 324/2 | 162/2  | 81/2 | 40/2 | 20/2 | 10/2 | 5/2 | 2/2 | 1
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
Res | 1    |  0    |    0   |  1   | 0    |  0   |  0   |  1  |  0  |

> Parte decimal

- 0.12 x 2 = 0.24 -> 0
- 0.24 x 2 = 0.48 -> 0
- 0.48 x 2 = 0.96 -> 0
- 0.96 x 2 = 1.92 -> 1
- 0.92 x 2 = 1.84 -> 1
- 0.84 x 2 = 1.68 -> 1
- 0.68 x 2 = 1.36 -> 1
- 0.36 x 2 = 0.72 -> 0

> Bin a Hex

 |0010 1000 1001|.|0001 1110|
 |:--:|:--:|:--:|
 | 2 8 9 |.| 1 E |
---
> Parte entera

Div | 84/2 | 42/2 | 21/2 | 10/2 | 5/2 | 2/2 | 1
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
Res | 0    |  0    |  1  |  0   | 1   |  0  | 

> Parte decimal

- 0.85 x 2 = 1.70 -> 1
- 0.70 x 2 = 1.40 -> 1
- 0.40 x 2 = 0.80 -> 0
- 0.80 x 2 = 1.60 -> 1
- 0.60 x 2 = 1.20 -> 1
- 0.20 x 2 = 0.40 -> 0
- 0.40 x 2 = 0.80 -> 0
- 0.80 x 2 = 1.60 -> 1

> Bin a Hex

 |0101 0100|.|1101 1001|
 |:--:|:--:|:--:|
 | 5 4 |.| D 9 |
---

## Respuestas
|Decimal | Binario | Hexadecimal
|:--:|:--:|:--:|
|649.12|1010001001.00011110₂|289.1E₁₆|
|84.85 |1010100.11011001₂|54.D9₁₆

---

### 2. Convertir del sistema hexadecimal a decimal y binario.(Método libre, pero hay que mencionar el método)

- CAE)16 =xxxx)2

> Sustitución directa por bloques de 4 bits

- C -> 1100
- A -> 1010
- E -> 1110

### CAE₁₆ = 110010101110₂

- A5B)16 = xxxx)2

> Sustitución directa por bloques de 4 bits

- A -> 1010
- 5 -> 0101
- B -> 1011

### A5B₁₆ = 101001011011₂
  
- 148.2C)16 =xxxx)10

> Expansión de base 16

|16^2|16^1|16^0|.|16^-1|16^-2
|:---:|:---:|:---:|:---:|:---:|:---:|
|1|4|8|.|2|C|
|256|64|8|.|2/16|12/256|

Suma: 328. 0.125 + 0.046875
### 148.2C)16 = 328.171875 
  
- CF.1D2₁₆ =xxxx)10

> Expansión de base 16

|16^1|16^0|.|16^-1|16^-2|16^-3
|:---:|:---:|:---:|:---:|:---:|:---:|
|C|F|.|1|D|2|
|192|15|.|1/16|13/256|2/4096

Suma: 207. 0.0625 + 0.05078125 + 0.0004882813
### CF.1D2₁₆ = 207.1137695313

---
## Respuestas
- CAE)16 = 110010101110₂
- A5B)16 = 101001011011₂
- 148.2C)16 = 328.171875₁₀
- CF.1D2)16 = 207.1137695313₁₀
---

### 3.- Convertir del sistema binario a los sistemas hexadecimal y decimal. Método libre

> Bin a Hex

|0001 0101|
|:--:|
| 1 5 |

> Bin a Dec

|2^7|2^6|2^5|2^4|2^3|2^2|2^1|2^0
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|0  |0  |0  |1  |0  |1  |0  |1
|0  |0  |0  |16 |0  |4  |0  |1

#### 16+4+1 = 21
---

> Bin a Hex

|0111 1011|.|1010|
|:--:|:--:|:--:|
| 7 B |.| A

> Bin a Dec

|2^6|2^5|2^4|2^3|2^2|2^1|2^0|.|2^-1|2^-2|2^-3|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1  |1  |1  |1  |0  |1  |1  |.|1  |0    |1
|64 |32 |16 |8  |0  |2  |1  |.|1/2|0    |1/8|

- 64+32+16+8+2+1 = 123
- 1/2+1/8 = 0.625

#### 123.625
---

> Bin a Hex

|1101|.|1100|
|:--:|:--:|:--:|
| D |.| C

> Bin a Dec

|2^3|2^2|2^1|2^0|.|2^-1|2^-2
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1  |1  |0  |1  |.|1   |1
|8  |4  |0  |1  |.|1/2 |1/4

- 8+4+1 = 13
- 1/2+1/4 = 0.75

#### 13.75
---

---
## Respuestas
|Binario | Hexadecimal |Decimal
|:--:|:--:|:--:|
|(10101)₂      |15₁₆  |21₁₀
|(1111011.101)₂|7B.A₁₆|123.625₁₀
|(1101.11)₂    |D.C₁₆ |13.75₁₀
---

## Tarea 1B

### a. Conversión entre sistemas numéricos
Desarrolle algoritmos que permitan realizar la conversión de números enteros y con parte fraccionaria entre los sistemas decimal, binario y hexadecimal, mostrando el procedimiento utilizado en cada conversión.
```python

```

### b. Detección de números pares e impares
Desarrolle un algoritmo que permita determinar si un número entero ingresado por el usuario es par o impar, mostrando el resulta
```python
def evaluar_paridad():
    try:
        numero = int(input("Ingrese un número entero: "))
        if numero % 2 == 0:
            print(f"El número {numero} es PAR.")
        else:
            print(f"El número {numero} es IMPAR.")
    except ValueError:
        print("Error: Debe ingresar un número entero válido.")

evaluar_paridad()
```

### c. Conversión de una dirección IPv4 a binario
Desarrolle un algoritmo que reciba una dirección IPv4 ingresada por el usuario y convierta cada uno de sus cuatro octetos de formato decimal a binario de 8 bits, mostrando la dirección IPv4 resultante en formato binario.
```python
def ipv4_binario():
    ip_string = input("Ingrese una dirección IPv4: ").strip()
    octetos = ip_string.split(".")
    
    if len(octetos) != 4:
        print("Formato IPv4 inválido.")
        return

    octetos_binarios = []
    for octeto in octetos:
        valor = int(octeto)
        # Formatear cada octeto a 8 bits rellenando con ceros a la izquierda
        bin_8bits = format(valor, '08b')
        octetos_binarios.append(bin_8bits)

    ip_binaria = ".".join(octetos_binarios)
    print(f"Salida: IPv4 en binario: {ip_binaria}")

ipv4_a_binario()
```

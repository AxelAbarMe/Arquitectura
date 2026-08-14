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
# Desarrolle algoritmos que permitan realizar la conversión de números enteros y con parte fraccionaria entre los sistemas decimal, binario y hexadecimal
# mostrando el procedimiento utilizado en cada conversión.

HEXADECIMAL_DIGITS = "0123456789ABCDEF"
PRECISION = 8  # Número de dígitos después del punto decimal para la parte fraccionaria

def decimal_a_binario(decimal, neg=False, testing=False):
    if decimal < 0:
        decimal = -decimal
        neg = True
    ent = int(decimal)
    frac = float(decimal - ent)
    binario = ""
    count = 0
    if not testing:
        print(f"--------------------------------")
    while ent > 0:
        if not testing:
            print(f"Paso ENT #{count + 1}: {ent} / 2 = {ent // 2} con residuo {ent % 2}")
        binario = str(ent % 2) + binario
        ent //= 2
        count += 1
    if frac > 0:
        if not testing:
            print(f"--------------------------------")
        binario += "."
        count = 0
        while frac > 0 and count < PRECISION:
            if not testing:
                print(f"Paso FRAC #{count + 1}: {frac:.2f} * 2 = {frac*2:.2f} con parte entera {int(frac * 2)}")
            frac *= 2
            bit = int(frac)
            binario += str(bit)
            frac -= bit
            count += 1
    if neg:
        binario = "-" + binario
    return binario if binario else "0"

def decimal_a_hexadecimal(decimal, neg=False, testing=False):
    if decimal < 0:
        decimal = -decimal
        neg = True
    ent = int(decimal)
    frac = float(decimal - ent)
    hexadecimal = ""
    count = 0
    if not testing:
        print(f"--------------------------------")
    while ent > 0:
        if not testing:
            print(f"Paso ENT #{count + 1}: {ent} / 16 = {ent // 16} con residuo {ent % 16}")
        residuo = ent % 16
        hexadecimal = HEXADECIMAL_DIGITS[residuo] + hexadecimal
        ent //= 16
    if frac > 0:
        if not testing:
            print(f"--------------------------------")
        hexadecimal += "."
        count = 0
        while frac > 0 and count < PRECISION//2:
            if not testing:
                print(f"Paso FRAC #{count + 1}: {frac:.4f} * 16 = {frac*16:.4f} con parte entera {int(frac * 16)}")
            frac *= 16
            bit = int(frac)
            hexadecimal += HEXADECIMAL_DIGITS[bit]
            frac -= bit
            count += 1
    if neg:
        hexadecimal = "-" + hexadecimal
    return hexadecimal if hexadecimal else "0"

def binario_a_decimal(binario, neg=False, testing=False):
    if binario < 0:
        binario = -binario
        neg = True
    ent = int(binario)
    frac = float(binario - ent)
    decimal = 0
    ent = str(ent)[::-1]  # Invertir la parte entera para facilitar el cálculo
    frac = round(frac,6)
    if not testing:
        print(f"--------------------------------")
    for i in range(len(ent)):
        decimal += int(ent[i]) * (2 ** i)
        if not testing:
            print(f"Paso ENT #{i + 1}: {ent[i]} * (2^{i}) = {int(ent[i]) * (2 ** i)} Resultado acumulado: {decimal}")
    frac = str(frac)[2:]  # Quitar "0." del inicio
    if not testing:
        print(f"--------------------------------")
    for i in range(len(frac)):
        decimal += int(frac[i]) * (2 ** -(i + 1))
        if not testing:
            print(f"Paso FRAC #{i + 1}: {frac[i]} * (2^-{i + 1}) = {int(frac[i]) * (2 ** -(i + 1))} Resultado acumulado: {decimal}")
    if neg:
        decimal = -decimal
    return decimal

def hexadecimal_a_decimal(hexadecimal, neg=False, testing=False):
    if hexadecimal.startswith("-"):
        hexadecimal = hexadecimal[1:]
        neg = True
    if "." in hexadecimal:
        ent, frac = hexadecimal.split(".")
    else:
        ent, frac = hexadecimal, ""
    decimal = 0
    ent = ent[::-1]  # Invertir la parte entera para facilitar el cálculo
    if not testing:
        print(f"--------------------------------")
    for i in range(len(ent)):
        decimal += HEXADECIMAL_DIGITS.index(ent[i]) * (16 ** i)
        if not testing:
            print(f"Paso ENT #{i + 1}: {ent[i]} * (16^{i}) = {HEXADECIMAL_DIGITS.index(ent[i]) * (16 ** i)} Resultado acumulado: {decimal}")
    if not testing:
        print(f"--------------------------------")
    for i in range(len(frac)):
        decimal += HEXADECIMAL_DIGITS.index(frac[i]) * (16 ** -(i + 1))
        if not testing:
            print(f"Paso FRAC #{i + 1}: {frac[i]} * (16^-{i + 1}) = {HEXADECIMAL_DIGITS.index(frac[i]) * (16 ** -(i + 1))} Resultado acumulado: {decimal}")
    if neg:
        decimal = -decimal
    decimal = round(decimal,4)
    return decimal

def binario_a_hexadecimal(binario, neg=False, testing=False):
    decimal = binario_a_decimal(float(binario), neg=neg, testing=testing)
    hexadecimal = decimal_a_hexadecimal(decimal, neg=neg, testing=testing)
    return hexadecimal

def hexadecimal_a_binario(hexadecimal, neg=False, testing=False):
    decimal = hexadecimal_a_decimal(hexadecimal, neg=neg, testing=testing)
    binario = decimal_a_binario(decimal, neg=neg, testing=testing)
    return binario

def test():
    assert decimal_a_binario(10.625, testing=True) == "1010.101"
    assert decimal_a_binario(-2.75, testing=True) == "-10.11"

    assert binario_a_decimal(1010.101, testing=True) == 10.625
    assert binario_a_decimal(-10.11, testing=True) == -2.75

    assert decimal_a_hexadecimal(255.254, testing=True) == "FF.4106"
    assert decimal_a_hexadecimal(-2754.5, testing=True) == "-AC2.8"

    assert hexadecimal_a_decimal("FF.4106", testing=True) == 255.254
    assert hexadecimal_a_decimal("-AC2.8", testing=True) == -2754.5

    assert binario_a_hexadecimal("1010.101", testing=True) == "A.A"
    assert binario_a_hexadecimal("-10.11", testing=True) == "-2.C"

    assert hexadecimal_a_binario("FF.4106", testing=True) == "11111111.01000001" 
    assert hexadecimal_a_binario("-AC2.8", testing=True) == "-101011000010.1"

    print("Todos los tests pasaron correctamente.")

def validate_input(value, base):
    try:
        if base == 2:
            int(value, 2)
        elif base == 10:
            float(value)
        elif base == 16:
            int(value, 16)
        return True
    except ValueError:
        return False

def menu():
    print("--------------------------------")
    print("Conversión de números entre sistemas decimal, binario y hexadecimal")
    print("Seleccione una opción:")
    print("1. Convertir decimal a binario")
    print("2. Convertir decimal a hexadecimal")
    print("3. Convertir binario a decimal")
    print("4. Convertir hexadecimal a decimal")
    print("5. Convertir binario a hexadecimal")
    print("6. Convertir hexadecimal a binario")
    print("7. Salir")
    return input("Ingrese el número de la opción deseada: ") 

def main():
    while True:
        opcion = menu()
        if opcion == "1":
            decimal_input = input('Ingrese un número decimal: ')
            if validate_input(decimal_input, 10):
                print(f"\nEl número en binario es: {decimal_a_binario(float(decimal_input))}")
            else:
                print('Entrada inválida. Por favor, ingrese un número decimal válido.')
        elif opcion == "2":
            decimal_input = input('Ingrese un número decimal: ')
            if validate_input(decimal_input, 10):
                print(f"El número en hexadecimal es: {decimal_a_hexadecimal(float(decimal_input))}")
            else:
                print('Entrada inválida. Por favor, ingrese un número decimal válido.')
        elif opcion == "3":
            binario_input = input('Ingrese un número binario: ')
            if validate_input(binario_input, 2):
                print(f"El número en decimal es: {binario_a_decimal(float(binario_input))}")
            else:
                print('Entrada inválida. Por favor, ingrese un número binario válido.')
        elif opcion == "4":
            hexadecimal_input = input('Ingrese un número hexadecimal: ')
            if validate_input(hexadecimal_input, 16):
                print(f"El número en decimal es: {hexadecimal_a_decimal(hexadecimal_input)}")
            else:
                print('Entrada inválida. Por favor, ingrese un número hexadecimal válido.')
        elif opcion == "5":
            binario_input = input('Ingrese un número binario: ')
            if validate_input(binario_input, 2):
                print(f"El número en hexadecimal es: {binario_a_hexadecimal(binario_input)}")
            else:
                print('Entrada inválida. Por favor, ingrese un número binario válido.')
        elif opcion == "6":
            hexadecimal_input = input('Ingrese un número hexadecimal: ')
            if validate_input(hexadecimal_input, 16):
                print(f"El número en binario es: {hexadecimal_a_binario(hexadecimal_input)}")
            else:
                print('Entrada inválida. Por favor, ingrese un número hexadecimal válido.')
        elif opcion == "7":
            print("Saliendo...")
            break
        else:
            print("Opción no válida. Por favor, ingrese un número válido.")

if __name__ == "__main__":
    test()
    main()
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
    ip_string = input("Ingrese una dirección IPv4 (ej. 192.168.1.1): ").strip()
    octetos = ip_string.split(".")
    
    if len(octetos) != 4:
        print("Error: El formato IPv4 debe tener exactamente 4 octetos separados por puntos.")
        return

    octetos_binarios = []
    for octeto in octetos:
        try:
            valor = int(octeto)
            
            # Valida el rango permitido de 0 a 255
            if not (0 <= valor <= 255):
                print(f"Error: El valor '{octeto}' está fuera de rango. Debe ser entre 0 y 255.")
                return
                
            # Formatear cada octeto a 8 bits rellenando con ceros a la izquierda
            bin_8bits = format(valor, '08b')
            octetos_binarios.append(bin_8bits)
            
        except ValueError:
            # Captura el error si el usuario ingresa letras o caracteres especiales
            print(f"Error: '{octeto}' no es un número entero válido.")
            return

    ip_binaria = ".".join(octetos_binarios)
    print(f"Salida: IPv4 en binario: {ip_binaria}")

ipv4_binario()

```

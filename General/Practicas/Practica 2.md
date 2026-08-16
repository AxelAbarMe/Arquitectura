# Práctica de Laboratorio
## Sistemas Numéricos, Códigos Binarios y Álgebra Booleana en Python

---

### Ejercicio 1: Complemento a 1

Desarrolle un algoritmo en Python que calcule el complemento a 1 de un número binario ingresado por el usuario como cadena de texto.

* a) El algoritmo debe validar que la cadena ingresada contenga únicamente los caracteres '0' y '1'.
* b) Debe invertir cada bit (0→1 y 1→0) sin necesidad de convertir el número a decimal.
* c) Debe mostrar el resultado conservando la misma cantidad de dígitos que el número original.
* d) Pruebe el algoritmo con los valores `101100` y `0011`.

---

### Ejercicio 2: Verificador de paridad

Desarrolle un algoritmo que reciba un byte de 8 bits (1 bit de paridad + 7 bits de datos) junto con el tipo de paridad esperado (par o impar), y determine si el byte llegó sin errores.

* a) El algoritmo debe contar la cantidad de unos presentes en todo el byte (incluyendo el bit de paridad).
* b) Debe comparar dicha cantidad contra el esquema de paridad indicado (par o impar) y determinar si es válido.
* c) Debe imprimir un mensaje indicando "Sin error de paridad" o "Error de paridad detectado".
* d) Pruebe el algoritmo con el byte `10110010` bajo esquema par, y con `01100011` bajo esquema impar.

---

### Ejercicio 3: Contador de bits en 1 (Peso de Hamming)

Desarrolle un algoritmo que reciba un número binario como cadena y cuente cuántos bits en 1 contiene.

* a) El algoritmo debe recorrer cada carácter de la cadena y determinar si es '1'.
* b) Debe llevar un contador que se incremente cada vez que encuentre un bit en 1.
* c) Al finalizar, debe mostrar el total de unos encontrados.
* d) Explique brevemente cómo se relaciona esta cantidad de unos con el cálculo del bit de paridad.

---

### Ejercicio 4: Conversión Octal ↔ Binario ↔ Decimal

Desarrolle un algoritmo que convierta un número octal ingresado por el usuario a su equivalente binario y decimal.

* a) El algoritmo debe solicitar un número en base 8 al usuario.
* b) Debe convertirlo primero a decimal, y luego a binario.
* c) Debe mostrar ambos resultados (decimal y binario) en pantalla.
* d) Pruebe el algoritmo con el número octal `572`.

---

### Ejercicio 5: Generador de tabla de verdad

Desarrolle un algoritmo que genere automáticamente la tabla de verdad de la expresión booleana F = A AND (B OR NOT C), para las 8 combinaciones posibles de A, B y C.

* a) El algoritmo debe generar todas las combinaciones posibles de 0 y 1 para las tres variables.
* b) Debe evaluar la expresión booleana para cada combinación.
* c) Debe imprimir los resultados en formato de tabla (A, B, C, F).
* d) Indique cuántas combinaciones de A, B y C hacen que F sea igual a 1.

---

## RESPUESTAS

---

### Ejercicio 1

```python
def complemento_a_1(binario):
    # validación de que solo contenga 0 y 1
    if not all(bit in "01" for bit in binario):
        return "Entrada inválida: solo se permiten 0 y 1"
    
    resultado = ""
    for bit in binario:
        if bit == "0":
            resultado += "1"
        else:
            resultado += "0"
    return resultado

# Pruebas
print(complemento_a_1("101100"))  # 010011
print(complemento_a_1("0011"))    # 1100
```

* a) Se valida con `all(bit in "01" for bit in binario)`.
* b) Se recorre cada carácter y se invierte directamente, sin pasar por decimal.
* c) El resultado conserva la misma longitud porque se recorre carácter por carácter.
* d) `101100` → `010011` ; `0011` → `1100`

---

### Ejercicio 2

```python
def verificar_paridad(byte, tipo_paridad):
    cantidad_unos = byte.count("1")
    
    if tipo_paridad == "par":
        valido = (cantidad_unos % 2 == 0)
    elif tipo_paridad == "impar":
        valido = (cantidad_unos % 2 != 0)
    else:
        return "Tipo de paridad no reconocido"
    
    if valido:
        return "Sin error de paridad"
    else:
        return "Error de paridad detectado"

# Pruebas
print(verificar_paridad("10110010", "par"))    # Sin error de paridad
print(verificar_paridad("01100011", "impar"))  # Sin error de paridad
```

* a) Se usa `byte.count("1")` para contar todos los unos del byte completo.
* b) Se aplica el operador módulo (`%2`) para determinar si la cantidad es par o impar.
* c) Se retorna el mensaje correspondiente según el resultado de la comparación.
* d) `10110010` tiene 4 unos (par) → cumple esquema par, sin error. `01100011` tiene 4 unos (par) → bajo esquema impar, sí presenta error de paridad.

---

### Ejercicio 3

```python
def contar_unos(binario):
    contador = 0
    for bit in binario:
        if bit == "1":
            contador += 1
    return contador

# Prueba
print(contar_unos("101100"))  # 3
```

* a) y b) Se recorre la cadena carácter por carácter, incrementando `contador` en cada '1' encontrado.
* c) Se retorna el valor final de `contador`.
* d) El bit de paridad se calcula precisamente a partir de esta cantidad de unos: si el protocolo es de paridad par, el bit de paridad se ajusta para que el total de unos (datos + paridad) sea par; si es impar, se ajusta para que el total sea impar.

---

### Ejercicio 4

```python
def octal_a_decimal_binario(octal):
    decimal = int(octal, 8)          # convierte directamente de base 8 a decimal
    binario = bin(decimal)[2:]       # convierte decimal a binario, eliminando el prefijo "0b"
    return decimal, binario

# Prueba
decimal, binario = octal_a_decimal_binario("572")
print("Decimal:", decimal)  # 378
print("Binario:", binario)  # 101111010
```

* a) Se solicita el número en base 8 (puede ingresarse con `input()`).
* b) Se usa `int(octal, 8)` para obtener el valor decimal.
* c) Se usa `bin()` para obtener el binario, y se muestran ambos resultados.
* d) `572₈` = `378₁₀` = `101111010₂`

---

### Ejercicio 5

```python
def evaluar_expresion(a, b, c):
    return a and (b or not c)

print("A B C | F")
contador_unos = 0
for a in [0, 1]:
    for b in [0, 1]:
        for c in [0, 1]:
            f = int(evaluar_expresion(bool(a), bool(b), bool(c)))
            print(f"{a} {b} {c} | {f}")
            if f == 1:
                contador_unos += 1

print("Cantidad de combinaciones donde F=1:", contador_unos)
```

* a) Se generan las 8 combinaciones con dos ciclos `for` anidados sobre los valores 0 y 1.
* b) Se evalúa la expresión usando los operadores lógicos `and`, `or` y `not` de Python.
* c) Se imprime cada fila en formato tabular (A, B, C, F).
* d) De las 8 combinaciones posibles, F=1 se cumple en 5 de ellas (todas excepto A=0 con cualquier B,C, es decir cuando A=1 y (B=1 o C=0)).

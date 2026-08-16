# Práctica 1
## Sistemas Binarios, Código ASCII y Álgebra Booleana

---

### Ejemplo 1

Un ingeniero explica a un grupo de estudiantes la diferencia entre los sistemas digitales y los sistemas analógicos.

* a) Explique la diferencia principal entre un sistema digital y un sistema analógico, en cuanto a los valores que manejan.
* b) A nivel físico, ¿qué representan el 1 y el 0 de un sistema digital?
* c) Mencione una ventaja del sistema digital frente al sistema analógico en cuanto a interferencias.
* d) Indique cuántos bits conforman un byte.

---

### Ejemplo 2

Convierta el número decimal 45 a su equivalente en binario y en hexadecimal, utilizando la técnica de división.

* a) Realice la conversión a binario, indicando el proceso de divisiones.
* b) Realice la conversión a hexadecimal, indicando el proceso de divisiones.
* c) Indique cuál bit obtenido en la conversión binaria corresponde al MSB y cuál al LSB.
* d) Explique por qué el procesador maneja mejor el hexadecimal que el binario al trabajar con registros y memorias internas.

---

### Ejemplo 3

Convierta los siguientes valores a decimal:

* a) El número binario 110101₂
* b) El número hexadecimal 3F₁₆
* c) Explique en qué consiste la técnica de "expansión de la base" utilizada para realizar estas conversiones.
* d) Indique qué diferencia existe entre realizar esta conversión de forma manual (software) y hacerlo por hardware, en cuanto a complejidad y tiempo.

---

### Ejemplo 4

Convierta el número decimal 18,625 a binario, incluyendo su parte fraccionaria.

* a) Convierta la parte entera (18) a binario utilizando el método de divisiones sucesivas.
* b) Convierta la parte fraccionaria (0,625) a binario multiplicando sucesivamente por 2.
* c) Escriba el resultado final combinando ambas partes.
* d) Explique brevemente el procedimiento para convertir la parte fraccionaria de un número decimal a hexadecimal (a diferencia de binario).

---

### Ejemplo 5

Se tienen los siguientes valores en distintos formatos de complemento.

* a) Calcule el complemento a 10 de 4720 (considerando 4 dígitos).
* b) Calcule el complemento a 1 de 1010₂.
* c) Calcule el complemento a 2 de 1010₂, a partir del resultado del inciso b).
* d) Explique por qué el complemento a 10 de 598 no es el mismo que el complemento a 10 de 0598.

---

### Ejemplo 6

Realice la siguiente resta utilizando el método de complemento a 2, propio del sistema computacional: 10110₂ − 01011₂

* a) Indique el complemento a 2 del valor que se va a restar (01011₂).
* b) Realice la suma correspondiente entre el minuendo y el complemento obtenido.
* c) Indique si existe acarreo y qué debe hacerse con él.
* d) Escriba el resultado final e indique si corresponde a un caso positivo o negativo.

---

### Ejemplo 7

Realice la suma en BCD de los valores 347 y 265.

* a) Escriba ambos números en su representación BCD (grupos de 4 bits por dígito).
* b) Realice la suma bit por bit de cada grupo, indicando en cuáles grupos fue necesario sumar 0110 (6) por generar un valor fuera del rango BCD (0-9).
* c) Escriba el resultado final en BCD y su equivalente decimal.
* d) Explique en qué consiste el paso 3 del procedimiento de suma BCD (corrección con 6).

---

### Ejemplo 8

Convierta el número binario 10110₂ a código Gray.

* a) Indique cuál es el primer dígito del código Gray resultante y por qué.
* b) Realice el procedimiento de suma en pares de dígitos para obtener el resto del código.
* c) Escriba el resultado final en código Gray.
* d) Explique la diferencia general entre el procedimiento de binario a Gray y el de Gray a binario.

---

### Ejemplo 9

Se reciben dos palabras de 8 bits (1 bit de paridad + 7 bits de datos), cada una bajo un esquema de paridad distinto.

- Esquema Par: [0] 1101001
- Esquema Impar: [1] 1000011

* a) Cuente la cantidad de unos presentes en los datos (sin incluir el bit de paridad) de cada palabra.
* b) Determine si cada palabra cumple correctamente con su esquema de paridad (par o impar), considerando el total de unos incluyendo el bit de paridad.
* c) Indique cuál de las dos palabras presenta un error de paridad, si lo hay.
* d) Explique para qué se utiliza el bit de paridad dentro de un protocolo de envío de datos.

---

### Ejemplo 10

En álgebra booleana se tienen las siguientes expresiones y compuertas.

* a) Simplifique la expresión F = x + xy, indicando la ley o propiedad booleana utilizada.
* b) Simplifique la expresión F = x(x + y), indicando la ley o propiedad booleana utilizada.
* c) Complete la tabla de verdad de una compuerta NAND (AND seguida de un inversor) para las combinaciones de entrada 0-0, 0-1, 1-0 y 1-1.
* d) Explique, en términos generales, por qué una compuerta NAND con entradas 0 y 0 genera una salida en 1 (relacione su respuesta con el comportamiento del transistor y el voltaje VCC).

---

## RESPUESTAS

---

### Ejemplo 1

* a) El sistema digital trabaja únicamente con dos valores (1 y 0), mientras que el sistema analógico puede tomar un valor y (continuo) para cada x, es decir, infinitos valores posibles.
* b) El 1 representa un voltaje alto (por ejemplo 5V o 3,3V, dependiendo del sistema) y el 0 representa 0V; es decir, "prendido" y "apagado".
* c) El sistema digital presenta inmunidad a interferencias, mientras que el sistema analógico sí se ve afectado por ellas.
* d) 8 bits.

---

### Ejemplo 2

* a) 45/2=22 r1, 22/2=11 r0, 11/2=5 r1, 5/2=2 r1, 2/2=1 r0, 1/2=0 r1. Leyendo los residuos de abajo hacia arriba: 45₁₀ = 101101₂
* b) 45/16=2 r13(D), 2/16=0 r2. Leyendo de abajo hacia arriba: 45₁₀ = 2D₁₆
* c) El MSB (bit más significativo) es el 1 de la izquierda (posición más alta); el LSB (bit menos significativo) es el 1 obtenido en la primera división, ubicado más a la derecha.
* d) Porque el hexadecimal permite representar la misma cantidad de información con menos dígitos, facilitando el manejo de registros y memorias, en comparación con las largas cadenas de binario.

---

### Ejemplo 3

* a) 110101₂ = 1·2⁵+1·2⁴+0·2³+1·2²+0·2¹+1·2⁰ = 32+16+0+4+0+1 = 53₁₀
* b) 3F₁₆ = 3·16¹+15·16⁰ = 48+15 = 63₁₀
* c) Consiste en multiplicar cada dígito del número por la base elevada a la posición que ocupa (empezando en 0 desde la derecha) y sumar todos los resultados obtenidos.
* d) De forma manual (software) toma más tiempo pero requiere menos componentes; por hardware, realizar múltiples divisiones (o los circuitos equivalentes) implica mayor complejidad en el diseño, aunque es más rápido.

---

### Ejemplo 4

* a) 18/2=9 r0, 9/2=4 r1, 4/2=2 r0, 2/2=1 r0, 1/2=0 r1 → 18₁₀ = 10010₂
* b) 0,625×2=1,25 → 1 ; 0,25×2=0,5 → 0 ; 0,5×2=1,0 → 1. Parte fraccionaria = 0,101₂
* c) 18,625₁₀ = 10010,101₂
* d) Se multiplica la parte fraccionaria sucesivamente por 16 (la base hexadecimal), y en cada paso se reserva el dígito entero resultante, repitiendo el proceso con la parte decimal restante hasta obtener la precisión deseada.

---

### Ejemplo 5

* a) 10⁴ − 4720 = 10000 − 4720 = 5280
* b) Complemento a 1 de 1010₂ = 0101₂ (se invierte cada bit)
* c) Complemento a 2 = 0101₂ + 1₂ = 0110₂
* d) Porque el complemento depende de la cantidad de dígitos (n) del número, ya que la fórmula es rⁿ − N; al agregar un cero a la izquierda (0598) se está usando un valor distinto de n, lo que cambia el resultado del complemento.

---

### Ejemplo 6

* a) Complemento a 2 de 01011₂: complemento a 1 = 10100₂, +1 = 10101₂
* b)
```
 10110
+10101
------
101011
```
* c) Sí existe acarreo (el bit más a la izquierda, sexto dígito); dicho acarreo se elimina.
* d) Resultado final: 01011₂ (caso positivo). Verificación: 10110₂=22, 01011₂=11, 22−11=11=01011₂, correcto.

---

### Ejemplo 7

* a) 347 → 0011 0100 0111 ; 265 → 0010 0110 0101
* b) Unidades: 0111+0101=1100 (fuera de rango BCD), se suma 0110 → 10010, se genera acarreo y queda 0010.
Decenas: 0100+0110=1010, +acarreo(1)=1011 (fuera de rango), se suma 0110 → 10001, se genera acarreo y queda 0001.
Centenas: 0011+0010=0101, +acarreo(1)=0110 (dentro de rango, no requiere corrección).
* c) Resultado BCD: 0110 0001 0010 = 612₁₀. Verificación: 347+265=612, correcto.
* d) Cuando la suma de dos dígitos BCD produce un resultado que no forma parte de la lista válida de BCD (mayor a 1001), se le suma 6 (0110) para corregirlo y obtener el dígito correcto, generando un acarreo hacia el siguiente grupo si corresponde.

---

### Ejemplo 8

* a) El primer dígito se mantiene igual al primer dígito del número binario original, que en este caso es 1.
* b) Sumando pares consecutivos de dígitos del binario original (1 0 1 1 0): 1⊕0=1, 0⊕1=1, 1⊕1=0, 1⊕0=1
* c) Código Gray resultante: 11101
* d) En la conversión binario→Gray se suman pares de dígitos del número original; en la conversión Gray→binario se suma el último dígito ya obtenido (del resultado binario) con el siguiente dígito del número Gray original, es decir, se usa el resultado que se va generando en cada paso.

---

### Ejemplo 9

* a) Esquema Par [0] 1101001: datos = 1,1,0,1,0,0,1 → 4 unos. Esquema Impar [1] 1000011: datos = 1,0,0,0,0,1,1 → 3 unos.
* b) Par: total de unos (parity + datos) = 0+4 = 4, cantidad par, cumple con el esquema Par (correcto). Impar: total de unos = 1+3 = 4, cantidad par, pero el esquema exige un total impar, por lo que no se cumple.
* c) La palabra con esquema Impar ([1] 1000011) presenta error de paridad.
* d) El bit de paridad se utiliza como mecanismo de detección de errores: permite verificar, al llegar la información, si la cantidad de unos en la trama coincide con la paridad (par o impar) acordada, y así detectar si ocurrió un error durante la transmisión.

---

### Ejemplo 10

* a) F = x + xy = x(1+y) = x·1 = x (ley de absorción)
* b) F = x(x+y) = xx + xy = x + xy = x (ley de absorción, aplicando además x·x=x)
* c)

| Entrada A | Entrada B | Salida NAND |
|:--:|:--:|:--:|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

* d) Cuando ambas entradas están en 0, el transistor de la compuerta no se enciende (no hay diferencia de voltaje que lo active), por lo que el voltaje que normalmente se restaría (cuando el transistor sí se enciende con VCC en 5V) se mantiene igual y se envía tal cual hacia la salida, generando así un 1 en la salida.

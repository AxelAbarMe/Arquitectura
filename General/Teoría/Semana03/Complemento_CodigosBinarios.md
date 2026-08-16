# Complemento

## Definición
El **complemento** es una técnica utilizada por el procesador para efectuar restas mediante sumas, ya que a nivel de hardware es mucho más sencillo y eficiente diseñar circuitos sumadores que circuitos restadores. En lugar de restar directamente, el procesador suma el complemento del sustraendo al minuendo.

* Es la base de cómo las computadoras representan y operan con números negativos.
* Permite que la Unidad Aritmético-Lógica (ALU) utilice el mismo circuito tanto para sumas como para restas.
* Existen distintos tipos de complemento según la base numérica utilizada: complemento a la base (radix complement) y complemento a la base disminuida (diminished radix complement).

### Fórmula general
> r^n - N

### Características
* **N**: Número al cual se le calculará el complemento.
* **n**: Cantidad de dígitos que se están considerando (incluyendo ceros a la izquierda si corresponde).
* **r**: Base del sistema numérico (10 para decimal, 2 para binario, 16 para hexadecimal, etc.).

**Diferencia importante:** El valor del complemento varía según la cantidad de dígitos considerados. Por ejemplo, 598 no otorga el mismo valor de complemento que 0598, ya que la cantidad de dígitos (n) cambia el resultado de r^n.

### Ejemplos

**Complemento a 10 de 598?** __402__

10³ - 598 = 1000 - 598 = **402**

> Aquí se usa n = 3, ya que 598 tiene 3 dígitos.

**Complemento a 10 de 0598?** __9402__

10⁴ - 0598 = 10000 - 598 = **9402**

> Aquí se usa n = 4, ya que se está considerando explícitamente el cero a la izquierda (0598 tiene 4 dígitos).

#### Ejemplo adicional: Complemento a 10 de 45
10² - 45 = 100 - 45 = **55**

#### Ejemplo adicional: Complemento a 10 de 0045
10⁴ - 0045 = 10000 - 45 = **9955**

> Nótese cómo, al aumentar la cantidad de dígitos considerados (n), el complemento resultante también cambia, a pesar de tratarse del mismo número.

---

## Complemento a 1

### Características
* También llamado **complemento a la base disminuida** en binario (r - 1 = 2 - 1 = 1).
* Procedimiento: se invierte cada bit del número, es decir, todos los 1 se convierten en 0 y todos los 0 se convierten en 1.
* Es equivalente a restar el número de una cadena de solo unos (1111...1) de la misma longitud.

### Ejemplos

**Complemento 1 de 110₂:** __001₂__

**Complemento 1 de 1110₂:** __0001₂__

#### Ejemplo adicional: Complemento 1 de 10101₂
* Número original: 10101
* Se invierte cada bit: 0 → 1, 1 → 0, 1 → 0, 0 → 1, 1 → 0
* Resultado: **01010₂**

#### Ejemplo adicional: Complemento 1 de 00110₂
* Número original: 00110
* Invirtiendo cada bit: **11001₂**

> Regla práctica: el complemento a 1 siempre es el "negativo lógico" bit a bit del número original; no requiere ningún cálculo aritmético, solo la inversión de cada posición.

---

## Complemento a 2

### Características
* También llamado **complemento a la base** en binario.
* Procedimiento: se obtiene primero el complemento a 1 del número, y luego se le suma 1₂ al resultado.
* Es el método más utilizado en los sistemas computacionales para representar números negativos, ya que permite que la suma y la resta se realicen con el mismo circuito y evita el problema del "doble cero" (+0 y -0) que presenta el complemento a 1.

### Ejemplos

**Complemento a 2 de 110₂:** 001₂ + 1₂ = **010₂**

**Complemento a 2 de 11110₂:** __00010₂__

> Se suma 1+1 = 0 y 1 de acarreo.

#### Ejemplo adicional: Complemento a 2 de 10101₂

1. Complemento a 1: 10101₂ → 01010₂
2. Se suma 1: 01010₂ + 1₂ = **01011₂**

**Resultado: 01011₂**

#### Ejemplo adicional: Complemento a 2 de 001100₂

1. Complemento a 1: 001100₂ → 110011₂
2. Se suma 1: 110011₂ + 1₂ = **110100₂**

**Resultado: 110100₂**

### Tabla comparativa: Complemento a 1 vs Complemento a 2

| Aspecto                     | Complemento a 1                     | Complemento a 2                              |
|:------------------------------|:---------------------------------------|:-----------------------------------------------|
| Procedimiento                | Se invierten todos los bits           | Complemento a 1 + 1                            |
| Representación del cero      | Existen +0 y -0 (dos representaciones) | Existe una única representación del cero        |
| Uso en hardware actual        | Poco común                            | Es el método estándar en procesadores actuales |
| Requiere ajuste de acarreo    | Sí (end-around carry)                 | No, el acarreo final simplemente se descarta   |

---

## Resta en sistema computacional

### Fórmula
> M - N + r^n

### Características
* **M = N** → El resultado es 0.
* **M > N** → M - N > 0 (resultado positivo).
* **M < N** → M - N < 0 (resultado negativo).

### Pasos
1. Igualar la cantidad de dígitos (bases) de ambos números, agregando ceros a la izquierda si es necesario.
2. Calcular el complemento a 2 del valor que se va a restar (el sustraendo, N).
3. Se realiza la suma entre el minuendo (M) y el complemento a 2 obtenido.
4. Se elimina el acarreo final, si existe, ya que este bit adicional no forma parte del resultado.

### Ejemplos

#### Caso positivo

**11010₂ - 010₂ = ____**

1. Igualar cantidad de dígitos sin que afecte el resultado del valor: 11010₂ - 00010₂
2. Complemento a 2 de 00010: **11110**
3. Se completa sumando ambos valores:
```
 11010
+11110
------
111000
```
4. Se elimina el acarreo final, completando el resultado: **11000₂**

---

**11111₂ - 1010₂ = ____**

* 11111₂ - 01010₂
* Complemento a 2 de 01010₂ = 10110₂
* 11111₂ + 10110₂
```
 11111
+10110
------
110101
```
* Se elimina el acarreo: **10101₂**

---

#### Ejemplo adicional (caso positivo): 1101₂ - 0011₂

1. Ambos números ya tienen la misma cantidad de dígitos (4 bits).
2. Complemento a 2 de 0011₂:
   * Complemento a 1: 1100₂
   * Se suma 1: 1100₂ + 1₂ = **1101₂**
3. Se suma:
```
 1101
+1101
-----
11010
```
4. Se elimina el acarreo: **1010₂**

> Comprobación en decimal: 1101₂ = 13, 0011₂ = 3, 13 - 3 = 10 = 1010₂ ✔

#### Caso negativo

**Nota:** El procesador guarda un bit con el complemento del resultado en caso de que este sea negativo, es decir, cuando no se genera acarreo en la suma final, el resultado obtenido se encuentra en complemento a 2 y debe volver a complementarse para leer su valor real, anteponiendo el signo negativo.

**110₂ - 10110₂ = ____**

1. Unificar bases: 00110₂ - 10110₂
2. Complemento a 2 de 10110₂: **01010₂** (el número que se resta es el que se complementa)
3. Se suma; no debe de haber acarreo en la suma cuando el resultado es negativo:
```
 00110
+01010
------
 10000     // Sin acarreo
```
4. Como no hubo acarreo, el resultado está en complemento a 2. Se vuelve a complementar: complemento a 2 de 10000₂ = 01111₂ + 1₂ = 10000₂
   Resultado final: **-10000₂**

---

**1010₂ - 111100₂ = ____**

* 001010₂ - 111100₂
* Complemento a 2 de 111100₂ = 000100₂
* 001010₂ + 000100₂
```
 001010
+000100
-------
 001110
```
* No hay acarreo, por lo tanto el resultado es negativo y está en complemento a 2. Se complementa: **110010₂**
* R/ **-110010₂**

---

#### Ejemplo adicional (caso negativo): 0101₂ - 1011₂

1. Ambos ya poseen la misma cantidad de dígitos (4 bits).
2. Complemento a 2 de 1011₂:
   * Complemento a 1: 0100₂
   * Se suma 1: 0100₂ + 1₂ = **0101₂**
3. Se suma:
```
 0101
+0101
-----
 1010
```
4. No hay acarreo → el resultado es negativo y está en complemento a 2. Se complementa 1010₂:
   * Complemento a 1: 0101₂
   * Se suma 1: 0101₂ + 1₂ = **0110₂**
5. Resultado final: **-0110₂**

> Comprobación en decimal: 0101₂ = 5, 1011₂ = 11, 5 - 11 = -6 = -0110₂ ✔

### Bit de signo
* n < 0 → b = 1 (el bit más significativo en 1 indica que el número es negativo)
* n > 0 → b = 0 (el bit más significativo en 0 indica que el número es positivo)

> El bit de signo corresponde siempre al bit ubicado más a la izquierda (bit más significativo) en la representación en complemento a 2, y permite identificar de forma inmediata si el valor almacenado es positivo o negativo, sin necesidad de complementar el número.

### Resumen del procedimiento de resta binaria

| Paso | Acción                                                        |
|:----:|:----------------------------------------------------------------|
| 1    | Igualar la cantidad de dígitos de M y N                        |
| 2    | Calcular el complemento a 2 de N (el sustraendo)                |
| 3    | Sumar M + complemento a 2 de N                                  |
| 4    | Si hay acarreo, se descarta y el resultado es positivo           |
| 5    | Si no hay acarreo, el resultado es negativo y está en complemento a 2 (se debe complementar nuevamente para leer su valor absoluto) |

---

# Códigos binarios

## Definición
Un **código binario** es un conjunto de n bits utilizado para representar, de forma sistemática, un conjunto de símbolos, caracteres, dígitos decimales u otra información. A diferencia de la representación binaria pura (posicional), un código binario asigna combinaciones específicas de bits a cada símbolo según una regla o convención definida, no necesariamente basada en el valor numérico.

* Ejemplos comunes de códigos binarios: BCD, código Gray, ASCII, código de exceso 3, entre otros.
* Cada código binario tiene un propósito específico: facilitar la visualización en displays, minimizar errores al cambiar de un valor a otro, representar caracteres alfanuméricos, etc.

## Decimal Codificado Binario (BCD)

### Definición
El **BCD (Binary Coded Decimal)** es un código binario que representa cada dígito decimal (del 0 al 9) de forma independiente, utilizando 4 bits por cada dígito. A diferencia del binario puro, el BCD **no convierte el número completo a binario**, sino que codifica cada dígito decimal por separado.

* Tiene un máximo de 10 combinaciones válidas por cada grupo de 4 bits (0000 a 1001), ya que solo representa los dígitos del 0 al 9.
* Las combinaciones de 4 bits del 1010 al 1111 (10 a 15 en decimal) **no son válidas** en BCD, ya que no corresponden a ningún dígito decimal.
* Es ampliamente utilizado en dispositivos como calculadoras, relojes digitales y displays de 7 segmentos, donde es más práctico trabajar dígito por dígito.

### Tabla BCD

| Sim. Dec | Dígito BCD |
|:--------:|:----------:|
| 0        | 0000       |
| 1        | 0001       |
| 2        | 0010       |
| 3        | 0011       |
| 4        | 0100       |
| 5        | 0101       |
| 6        | 0110       |
| 7        | 0111       |
| 8        | 1000       |
| 9        | 1001       |

> Ejemplo de codificación de un número completo: el número decimal **27** en BCD se representa como **0010 0111** (2 = 0010, 7 = 0111), agrupando cada dígito por separado, a diferencia del binario puro donde 27 = 11011.

### Suma BCD

#### Pasos
1. Se transforma cada dígito decimal en su equivalente BCD.
2. Se suma bit por bit, agrupando en bloques de 4 bits (por cada dígito decimal).
3. Si el resultado de un bloque no forma parte de la lista de combinaciones válidas de BCD (es decir, es mayor a 1001 o generó acarreo hacia una posición inexistente), al resultado se le suma **0110 (6)** para corregirlo y obtener el valor BCD correcto.
4. Si al sumar 6 se genera un acarreo, este se envía hacia el siguiente bloque de 4 bits a la izquierda.
5. Se repite el paso 3 para cada bloque, hasta llegar al bloque más a la izquierda.

> La razón de sumar 6 al corregir es que existen 6 combinaciones "inválidas" (1010 a 1111) entre el final del rango BCD (1001) y el inicio del siguiente múltiplo de 16; sumar 6 "salta" esas combinaciones inválidas y ajusta el resultado al valor decimal correcto.

#### Ejemplo 1
```
 189
+286
-------
 475


 o bien


 0001 1000 1001
+0010 1000 0110
------------------


Equivalencias:


 0001  1000  1001
+0010  1000  0110
------------------
 0011 10000 1111
+           0110
          --------
          [1]0101   Y se genera acarreo
      10001
  +    0110
    -------
    [1]0111    Y se genera acarreo
0011
+  1
----
0100


 4    7   5




Nota: Si se pasa de 9, se corrige sumando 6
```

#### Ejemplo 2
```
 988
+889
-----
 1877


 o bien


 0001 1000 0111 0111




 1001   1000   1000
+1000   1000   1001
------------------------
10001 10000 10001
+            0110
           --------
           [1]0111   Y se genera acarreo
      10001
  +    0110
    -------
    [1]0111    Y se genera acarreo
10010
+0110
-----
[1]1000   Y se genera acarreo


1


1   8    7    7


```

#### Ejemplo 3
```
 890
+111
-----
1001


 o bien


0001 0000 0000 0001




 1000   1001   0000
+0001   0001   0001
------------------------
 1001   1010   0001
   +    0110
     -------
     [1]0000    Y se genera acarreo
 1010
+0110
-----
[1]0000   Y se genera acarreo


1


1    0     0     1


```

#### Ejemplo 4 (adicional): 45 + 38 en BCD

Conversión a BCD: 45 = 0100 0101; 38 = 0011 1000

```
 0100  0101
+0011  1000
-----------
 0111  1101
```

El bloque de la derecha (1101 = 13) no es válido en BCD, por lo que se corrige sumando 0110:
```
 1101
+0110
-----
[1]0011
```
Se genera acarreo, el cual se suma al bloque izquierdo:
```
 0111
+   1
-----
 1000
```

Resultado final: 1000 0011 → **83**

> Comprobación: 45 + 38 = 83 ✔

### Resumen del algoritmo de suma BCD

| Situación                                      | Acción                              |
|:-------------------------------------------------|:---------------------------------------|
| El resultado del bloque es ≤ 1001 y sin acarreo | El bloque es correcto, no se corrige   |
| El resultado del bloque es > 1001 o genera acarreo | Se suma 0110 (6) para corregir         |
| Al sumar 0110 se genera un nuevo acarreo         | Se envía el acarreo al bloque siguiente (izquierda) |

---

## Código de Gray

### Definición
El **código Gray** (también llamado código binario reflejado) es un sistema de codificación en el cual dos valores consecutivos difieren únicamente en **un solo bit**. Esto lo diferencia del binario puro, donde al pasar de un número a otro pueden cambiar varios bits simultáneamente.

* Es muy utilizado en sistemas de conteo, encoders rotativos y sensores de posición, ya que minimiza los errores que podrían producirse si varios bits cambiaran al mismo tiempo (lo cual, en un sistema físico real, no ocurre de forma perfectamente simultánea y puede generar lecturas erróneas transitorias).
* No es un código posicional, es decir, no se puede calcular su valor decimal multiplicando cada bit por una potencia de la base, como sí ocurre con el binario puro.

### Tabla Código de Gray

| C. Gray | Equiv Dec. |
|:-------:|:----------:|
| 0000    | 0          |
| 0001    | 1          |
| 0011    | 2          |
| 0010    | 3          |
| 0110    | 4          |
| 0111    | 5          |
| 0101    | 6          |
| 0100    | 7          |
| 1100    | 8          |
| 1101    | 9          |
| 1111    | 10         |
| 1110    | 11         |
| 1010    | 12         |
| 1011    | 13         |
| 1001    | 14         |
| 1000    | 15         |

> Nótese que entre cada fila consecutiva de la tabla, solamente cambia un bit; por ejemplo, entre 0001 (1) y 0011 (2) solo cambia el segundo bit desde la izquierda.

### Binario → Gray

#### Pasos
1. Se mantiene el primer dígito (bit más significativo) igual.
2. Se suma cada par de dígitos consecutivos del número binario original (bit actual XOR bit anterior), de izquierda a derecha.
3. Se repite el proceso hasta recorrer todos los dígitos.

> En términos formales, esto corresponde a la operación XOR entre cada bit y el bit inmediatamente a su izquierda: G(i) = B(i) XOR B(i-1), donde el primer bit de Gray es igual al primer bit binario.

#### Ejemplo
```
Número 1100₂

Proceso:

1. Se mantiene primer dígito: 1
2. Se realiza la suma de pares

1 + 1 + 0 + 0
|  \   \   \
1   0   1   0

Primer par: 11 = 0
Segundo par: 10 = 1
Tercer par: 00 = 0

3. Finalizar y juntar todos los dígitos obtenidos: 1010

Resultado final:
1100₂
|
v
1010 (G)
```

#### Ejemplo Gráfico
<img src="../../../img/Bin-Gray.png" Alt="Binario a Gray" Width=500>

#### Ejemplo adicional: 1011₂ a Gray

1. Se mantiene el primer dígito: **1**
2. Se suman los pares consecutivos:
   * 1 y 0 → 1 XOR 0 = **1**
   * 0 y 1 → 0 XOR 1 = **1**
   * 1 y 1 → 1 XOR 1 = **0**
3. Resultado: **1110 (G)**

#### Ejemplo adicional: 0101₂ a Gray

1. Se mantiene el primer dígito: **0**
2. Se suman los pares:
   * 0 y 1 → **1**
   * 1 y 0 → **1**
   * 0 y 1 → **1**
3. Resultado: **0111 (G)**

### Gray → Binario

#### Pasos
1. Se mantiene el primer dígito (bit más significativo) igual.
2. Se suma el último dígito binario ya obtenido con el siguiente dígito del número Gray original.
3. Se repite el proceso hasta recorrer todos los dígitos.

> Formalmente: B(i) = G(i) XOR B(i-1), es decir, cada bit binario se obtiene sumando (XOR) el bit de Gray actual con el bit binario ya calculado en la posición anterior.

#### Ejemplo
```
Número 1111₂

Proceso:

1. Se mantiene primer dígito: 1
2. Se realiza la suma de los pares

Primer par: 11 = 0
Segundo par: 01 = 1
Tercer par: 11 = 0

1  1  1  1
|+/|+/|+/|
1  0  1  0

3. Finalizar y juntar todos los dígitos obtenidos: 1010

Resultado final:
1111 (G)
|
v
1010₂
```

#### Ejemplo Gráfico
<img src="../../../img/Gray-Bin.png" Alt="Gray a Binario" Width=500>

#### Ejemplo adicional: 1001 (G) a Binario

1. Se mantiene el primer dígito: **1**
2. Se calcula cada bit siguiente con el bit binario anterior ya obtenido:
   * 1 (bit binario anterior) XOR 0 (siguiente bit Gray) = **1**
   * 1 (bit binario anterior) XOR 0 (siguiente bit Gray) = **1**
   * 1 (bit binario anterior) XOR 1 (siguiente bit Gray) = **0**
3. Resultado: **1110₂**

#### Ejemplo adicional: 0110 (G) a Binario

1. Se mantiene el primer dígito: **0**
2. Se calcula cada bit siguiente:
   * 0 XOR 1 = **1**
   * 1 XOR 1 = **0**
   * 0 XOR 0 = **0**
3. Resultado: **0100₂**

### Comparación Binario puro vs Código Gray

| Característica                        | Binario puro                     | Código Gray                          |
|:-----------------------------------------|:------------------------------------|:-----------------------------------------|
| Cambios de bit entre valores consecutivos| Puede cambiar más de un bit         | Cambia exactamente un bit                |
| Es posicional (valor por potencias)       | Sí                                   | No                                       |
| Uso típico                               | Aritmética, almacenamiento general  | Encoders, sensores de posición, conteo   |
| Riesgo de error de lectura en hardware    | Mayor, por cambios simultáneos de bits | Menor, por cambio de un solo bit a la vez |

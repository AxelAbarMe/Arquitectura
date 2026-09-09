# RESUMEN: ARQUITECTURA DE COMPUTADORAS

## 1. Sistemas Numéricos (Sistemas Binarios)

### Sistema Digital vs Analógico
- **Sistema digital**: Conjunto de elementos que procesan señales discretas (0 y 1) para producir salidas; alta inmunidad al ruido; niveles de voltaje (~5V o 3.3V = 1, 0V = 0).
- **Sistema analógico**: Señal continua en el tiempo; para cada entrada x existe una salida y; más susceptible a interferencias (ej. termómetro de mercurio, radio AM/FM).
- **Tabla comparativa**: Digital = valores discretos, alta inmunidad al ruido, alta precisión (computadoras, relojes digitales); Analógico = valores continuos, baja inmunidad, depende de la calidad de la señal (termómetro, radio).

### Almacenamiento
- **Bit**: Unidad mínima de información (0 o 1).
- **Byte**: Conjunto de 8 bits, unidad básica para representar caracteres.
- **Ancho de banda**: Datos transmitidos por tiempo, medido en Megabit (Mb); diferente del tamaño de almacenamiento, medido en Megabyte (MB), Gigabyte (GB) o Terabyte (TB).
- **Equivalencias**: 1 byte = 8 bits | 1 KB = 1024 bytes | 1 MB = 1024 KB | 1 GB = 1024 MB | 1 TB = 1024 GB.
- **Nota importante**: No confundir Mb (velocidad de conexión) con MB (tamaño de archivo); 100 Mb/s ≈ 12.5 MB/s.

### Sistemas Numéricos
- **Binario (base 2)**: Dígitos 0 y 1.
- **Octal (base 8)**: Dígitos 0 al 7.
- **Decimal (base 10)**: Dígitos 0 al 9, uso convencional.
- **Hexadecimal (base 16)**: Dígitos 0-9 y A-F (A=10 … F=15); un dígito = 4 bits (nibble), facilita lectura de binarios largos y reduce errores (ej. `#FF5733`).

### Tabla de Equivalencias (Bin - Hex - Dec)
| Bin | Hex | Dec | Bin | Hex | Dec |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 0000 | 0 | 0 | 1000 | 8 | 8 |
| 0001 | 1 | 1 | 1001 | 9 | 9 |
| 0010 | 2 | 2 | 1010 | A | 10 |
| 0011 | 3 | 3 | 1011 | B | 11 |
| 0100 | 4 | 4 | 1100 | C | 12 |
| 0101 | 5 | 5 | 1101 | D | 13 |
| 0110 | 6 | 6 | 1110 | E | 14 |
| 0111 | 7 | 7 | 1111 | F | 15 |

### Conversión Decimal → Binario/Hexadecimal (División Sucesiva)
- Se divide el número entre la base (2 o 16) sucesivamente hasta que el cociente sea menor que el divisor.
- Los residuos, leídos de **abajo hacia arriba**, forman el número convertido.
- **MSB**: primer bit (izquierda), corresponde al último residuo. **LSB**: último bit (derecha), corresponde al primer residuo.

**Ejemplo: 20 a binario**
| División | Cociente | Residuo |
|:--:|:--:|:--:|
| 20÷2 | 10 | 0 (LSB) |
| 10÷2 | 5 | 0 |
| 5÷2 | 2 | 1 |
| 2÷2 | 1 | 0 |
| 1 (fin) | - | 1 (MSB) |

→ **20 = 10100₂**

**Ejemplo: 200 a hexadecimal**
| División | Cociente | Residuo |
|:--:|:--:|:--:|
| 200÷16 | 12 | 8 |
| 12 (fin) | - | C |

→ **200 = C8₁₆**

- **Costo de la división sucesiva**: en hardware implica mayor complejidad de circuitos; en software implica mayor tiempo de procesamiento (cada división es una instrucción).

### Conversión Binario/Hexadecimal → Decimal (Expansión en potencias)
- Se multiplica cada dígito por la base elevada a su posición (desde 0, derecha a izquierda) y se suman los resultados.

**Ejemplo binario 1011**: 1×2³ + 0×2² + 1×2¹ + 1×2⁰ = 8+0+2+1 = **11**

**Ejemplo hexadecimal FA**: F(15)×16¹ + A(10)×16⁰ = 240+10 = **250**

### Conversión de Números con Parte Fraccionaria
- **Parte entera**: divisiones sucesivas (como arriba).
- **Parte fraccionaria**: se multiplica repetidamente por la base, tomando en cada paso la parte entera resultante.

**Ejemplo: 0.38 a binario** (multiplicar por 2)
- 0.38×2=0.76 → 0
- 0.76×2=1.52 → 1
- 0.52×2=1.04 → 1
- 0.04×2=0.08 → 0
- 0.08×2=0.16 → 0

→ **0.38 ≈ 0.01100₂** (250.38 ≈ 11111010.01100₂)

**Ejemplo: 0.38 a hexadecimal** (multiplicar por 16)
- 0.38×16=6.08 → 6
- 0.08×16=1.28 → 1
- 0.28×16=4.48 → 4

→ **250.38 ≈ FA.614₁₆**

### Conversión Binario ↔ Hexadecimal (método rápido)
- **Binario → Hex**: agrupar en bloques de 4 bits desde la derecha; rellenar con ceros a la izquierda si el último grupo queda incompleto.
  - Ejemplo: `10111011` → `1011 1011` → **BB**
  - Ejemplo con relleno: `101101` → `0010 1101` → **2D**
- **Hex → Binario**: cada dígito se expande directamente a 4 bits.
  - Ejemplo: `3E` → 3=0011, E=1110 → **00111110₂**

> [Resumen Completo](https://github.com/AxelAbarMe/Arquitectura/blob/main/General/Teor%C3%ADa/Semana02/SistemasN%C3%BAmericos.md) - Sistemas de numeración y sus conversiones.

---

## 2. Complemento y Códigos Binarios

### Complemento
- **Definición**: Técnica que permite al procesador restar mediante sumas, ya que los circuitos sumadores son más simples que los restadores; permite que la ALU use el mismo circuito para sumar y restar.
- **Fórmula general**: rⁿ - N, donde N=número, n=cantidad de dígitos, r=base.
- El valor del complemento cambia según la cantidad de dígitos (n) considerados: complemento a 10 de 598 (n=3) → 402; complemento a 10 de 0598 (n=4) → 9402.

### Complemento a 1
- También llamado complemento a la base disminuida (r-1=1 en binario).
- Se invierte cada bit (1→0, 0→1); equivale a restar de una cadena de puros unos.
- Presenta doble representación del cero (+0 y -0).
- Ejemplo: complemento 1 de `10101₂` → **01010₂**.

### Complemento a 2
- También llamado complemento a la base; se obtiene sumando 1 al complemento a 1.
- Método estándar en procesadores actuales; evita el doble cero y no requiere ajuste de acarreo (end-around carry).
- Ejemplo: complemento a 2 de `10101₂`: comp. a 1 = `01010₂`, +1 = **01011₂**.

### Tabla comparativa Complemento 1 vs 2
| Aspecto | Complemento a 1 | Complemento a 2 |
|:--|:--|:--|
| Procedimiento | Invertir bits | Comp. a 1 + 1 |
| Representación del 0 | +0 y -0 | Única |
| Uso actual | Poco común | Estándar |
| Ajuste de acarreo | Sí | No (se descarta) |

### Resta en Sistema Computacional
- **Fórmula**: M - N + rⁿ
- **Pasos**: 1) Igualar cantidad de dígitos. 2) Calcular complemento a 2 de N (sustraendo). 3) Sumar M + comp. a 2 de N. 4) Si hay acarreo, se descarta (resultado positivo); si no hay acarreo, el resultado es negativo y está en complemento a 2 (se debe volver a complementar para leer su valor real).

**Ejemplo caso positivo: 1101₂ - 0011₂**
- Comp. a 2 de 0011₂ = 1101₂
- 1101 + 1101 = 11010 → se descarta el acarreo → **1010₂** (= 10 decimal ✔ 13-3=10)

**Ejemplo caso negativo: 0101₂ - 1011₂**
- Comp. a 2 de 1011₂ = 0101₂
- 0101 + 0101 = 1010 (sin acarreo → resultado negativo en comp. a 2)
- Se complementa 1010₂ → 0110₂ → **Resultado: -0110₂** (=-6 decimal ✔ 5-11=-6)

- **Bit de signo**: bit más significativo; 1 = negativo, 0 = positivo.

### Códigos Binarios
- **Definición**: Conjunto de n bits que representa símbolos según una convención (no necesariamente el valor posicional). Ejemplos: BCD, Gray, ASCII, exceso 3.

### BCD (Binary Coded Decimal)
- Codifica cada dígito decimal (0-9) con 4 bits independientes; NO convierte el número completo a binario puro.
- Combinaciones 1010-1111 (10-15) **no son válidas** en BCD.
- Ejemplo: 27 en BCD = `0010 0111` (a diferencia del binario puro 27=`11011`).

**Tabla BCD**
| Dec | BCD | Dec | BCD |
|:--:|:--:|:--:|:--:|
| 0 | 0000 | 5 | 0101 |
| 1 | 0001 | 6 | 0110 |
| 2 | 0010 | 7 | 0111 |
| 3 | 0011 | 8 | 1000 |
| 4 | 0100 | 9 | 1001 |

### Suma BCD (pasos)
1. Convertir cada dígito decimal a BCD.
2. Sumar bit por bit en bloques de 4.
3. Si el bloque resulta >1001 o genera acarreo hacia una posición inexistente, sumar **0110 (6)** para corregir.
4. Si al sumar 6 se genera nuevo acarreo, se envía al siguiente bloque a la izquierda.
5. Repetir hasta el bloque más a la izquierda.

**Ejemplo: 45 + 38 en BCD**
- 45=`0100 0101`, 38=`0011 1000`
- Suma: `0111 1101` → el bloque derecho (1101=13) es inválido → +0110 → `[1]0011`, genera acarreo
- Acarreo al bloque izquierdo: `0111+1=1000`
- Resultado: `1000 0011` → **83** (✔ 45+38=83)

### Código de Gray
- Dos valores consecutivos difieren en **un solo bit**; usado en encoders y sensores de posición para evitar errores de lectura por cambios simultáneos de varios bits.
- No es posicional (no se calcula por potencias de la base).

**Tabla Código de Gray (parcial)**
| Gray | Dec | Gray | Dec |
|:--:|:--:|:--:|:--:|
| 0000 | 0 | 1100 | 8 |
| 0001 | 1 | 1101 | 9 |
| 0011 | 2 | 1111 | 10 |
| 0010 | 3 | 1110 | 11 |
| 0110 | 4 | 1010 | 12 |
| 0111 | 5 | 1011 | 13 |
| 0101 | 6 | 1001 | 14 |
| 0100 | 7 | 1000 | 15 |

### Binario → Gray (pasos)
1. Se mantiene el primer bit (MSB) igual.
2. Cada bit siguiente = XOR entre el bit actual y el bit anterior del binario original: G(i) = B(i) XOR B(i-1).

**Ejemplo: 1011₂ → Gray**
- Primer bit: 1
- 1 XOR 0 = 1 | 0 XOR 1 = 1 | 1 XOR 1 = 0
→ **1110 (Gray)**

### Gray → Binario (pasos)
1. Se mantiene el primer bit igual.
2. Cada bit binario siguiente = XOR entre el bit Gray actual y el bit binario ya calculado: B(i) = G(i) XOR B(i-1).

**Ejemplo: 1001 (Gray) → Binario**
- Primer bit: 1
- 1 XOR 0=1 | 1 XOR 0=1 | 1 XOR 1=0
→ **1110₂**

### Comparación Binario puro vs Gray
| Característica | Binario puro | Código Gray |
|:--|:--|:--|
| Cambios entre consecutivos | Puede cambiar +1 bit | Cambia exactamente 1 bit |
| Posicional | Sí | No |
| Uso típico | Aritmética general | Encoders, sensores |
| Riesgo de error hardware | Mayor | Menor |

> [Resumen Completo](https://github.com/AxelAbarMe/Arquitectura/blob/main/General/Teor%C3%ADa/Semana03/Complemento_CodigosBinarios.md) - Complemento a 1, complemento a 2 y códigos binarios.

---

## 3. Código ASCII y Álgebra Booleana

### ASCII
- **Definición**: Estándar que asigna un valor binario a cada carácter (letras, números, signos, controles), permitiendo que todos los dispositivos "hablen el mismo idioma".
- **ASCII estándar**: 7 bits → 2⁷=128 caracteres (0-127).
- **ASCII extendido**: 8 bits (incluye bit de paridad) → hasta 256 combinaciones.

### Estructura de la tabla ASCII
- Se combinan los 3 bits más significativos (b7 b6 b5 = "columna"/grupo) con los 4 bits restantes (b4 b3 b2 b1 = fila dentro del grupo).

| b7 b6 b5 | Rango de caracteres |
|:--:|:--|
| 000 | Caracteres de control |
| 001 | Puntuación y símbolos |
| 010 | Dígitos y símbolos |
| 011 | Dígitos 0-9 y símbolos adicionales |
| 100 | Mayúsculas A-O |
| 101 | Mayúsculas P-Z y símbolos |
| 110 | Minúsculas a-o |
| 111 | Minúsculas p-z y símbolos |

- Ejemplo: `1000001` = A; `1000010` = B (al incrementar los bits menos significativos se avanza al siguiente carácter).

### Bit de Paridad
- Mecanismo de detección de errores: se agrega un bit adicional para que la cantidad total de unos cumpla una condición (par o impar).
- **Par**: si la cantidad de 1's ya es par, el bit de paridad = 0; si es impar, = 1.
- **Impar**: si la cantidad de 1's ya es impar, el bit = 0; si es par, = 1.
- **Limitación**: solo detecta errores si se altera una cantidad **impar** de bits; si se alteran 2 bits simultáneamente, no se detecta.

**Ejemplo (paridad par)**: datos `1010011` (4 unos, par) → bit de paridad=0 → `[0]1010011`

### Álgebra Booleana
- **Simular**: imitación por software, sin replicar el funcionamiento interno.
- **Emular**: reproduce el comportamiento usando recursos reales de CPU (más fiel).
- **Ventaja de simplificar expresiones**: reduce costo de fabricación, consumo de energía, tamaño físico, probabilidad de fallas y mejora la velocidad de respuesta (menos etapas).

### Operadores Booleanos Básicos
| Operador | Notación | Resultado 1 cuando... |
|:--:|:--:|:--|
| AND | xy o x·y | Ambas entradas son 1 |
| OR | x+y | Al menos una entrada es 1 |
| NOT | x' | La entrada original es 0 |

**Tabla AND**: 0·0=0, 0·1=0, 1·0=0, 1·1=1
**Tabla OR**: 0+0=0, 0+1=1, 1+0=1, 1+1=1

- **VCC**: fuente de voltaje que alimenta el circuito (5V clásico, 3.3V moderno).
- **Ground (GND)**: referencia de 0V; necesario para cerrar el circuito y que la corriente complete su recorrido.
- **x + 0 = x**: propiedad de identidad de la OR.
- **Letargo/delay**: tiempo de propagación de la señal a través de una compuerta; nunca es instantáneo.

### Compuerta NAND (universal)
- Tabla de verdad: negación de la AND → 00→1, 01→1, 10→1, 11→0.
- Es universal: cualquier compuerta (AND, OR, NOT) puede construirse solo con NAND.
- A nivel de transistores: solo genera 0 cuando ambas entradas están en 1 (ambos transistores conducen); con ambas entradas en 0, ningún transistor conduce y el voltaje de VCC se refleja como 1 en la salida.

| Compuerta | Universal | Motivo |
|:--:|:--:|:--|
| NAND | Sí | Puede formar todas las demás combinándose |
| NOR | Sí | Igual que NAND |
| AND | No | No genera NOT por sí sola |
| OR | No | No genera NOT por sí sola |

### Half Adder
- Circuito combinacional que suma 2 bits, produciendo Suma (S) y Acarreo (C).
- **Tabla de verdad**: 00→S0,C0 | 01→S1,C0 | 10→S1,C0 | 11→S0,C1.
- **Fórmulas**: S = x ⊕ y (XOR); C = x·y (AND).

### Diferencia Half Adder vs Full Adder
| Característica | Half Adder | Full Adder |
|:--|:--|:--|
| Entradas | 2 bits (x,y) | 3 bits (x,y,Cin) |
| Uso | Bit menos significativo | Resto de bits (con acarreo previo) |

### Circuitos Integrados por Compuerta
| Compuerta | CI | Entradas | Compuertas/chip |
|:--:|:--:|:--:|:--:|
| AND | 74LS08 | 2 | 4 |
| OR | 4071 | 2 | 4 |
| NOT | 74LS04 | 1 | 6 |
| XOR | 7486 | 2 | 4 |
| XNOR | 4077 | 2 | 4 |
| NAND | 74LS00 | 2 | 4 |
| NOR | 74LS02 | 2 | 4 |

> [Resumen Completo](https://github.com/AxelAbarMe/Arquitectura/blob/main/General/Teor%C3%ADa/Semana04/ASCII-AlgBool.md) - Código ASCII y Álgebra Booleana (con DigitalWork).

---

## 4. Diseño Lógico

### Conceptos Base
- **Tabla de verdad**: revisa todas las combinaciones posibles; 2^x combinaciones según x variables.
- **Minitérmino**: término con variables normales (=1) o complementadas (=0). Ej: m5 = xy'z → 101.
- **Maxitérmino**: término con variables normales (=0) o complementadas (=1). Ej: M5 = x'+y+z' → 101.

### Ejemplo de Simplificación (2 variables)
Condición: "Si el segundo está de acuerdo" o "si ambos están de acuerdo" → F(x,y) = Σm(1,3)
- F = m1+m3 = x'y+xy = y(x'+x) = y·1 = **y**

### Ejemplo de Simplificación (3 variables)
F(x,y,z) = Σm(5,6,7)
- = xy'z + xyz' + xyz
- = xy'z + xy(z+z') = xy'z + xy = x(y'z+y) = x(y+y')(y+z) = x(y+z)
- = **xy + xz**

- **ASIC**: chip diseñado específicamente para una función; mejora el delay y reduce costos al simplificar componentes.

### Mapa de Karnaugh — Fundamentos
- Herramienta gráfica que representa visualmente el álgebra booleana para simplificar expresiones.
- **Celdas vecinas (adyacentes)**: difieren en un solo bit. Ej: 00-01 son vecinas; 00-10 son vecinas; **00-11 NO son vecinas**.
- Se usa **código Gray** en el orden de filas/columnas (00,01,11,10) para garantizar un solo cambio de bit entre celdas adyacentes (incluidos los bordes/wraparound).

### Mapa K de 2 Variables (formato)
| x\y | y' (0) | y (1) |
|:--:|:--:|:--:|
| **x' (0)** | m0 | m1 |
| **x (1)** | m2 | m3 |

### Mapa K de 3 Variables (formato)
| x \ yz | y'z' (00) | y'z (01) | yz (11) | yz' (10) |
|:--:|:--:|:--:|:--:|:--:|
| **x' (0)** | m0 | m1 | m3 | m2 |
| **x (1)** | m4 | m5 | m7 | m6 |

**Ejemplo**: F(x,y,z)=Σ(0,3,4,7)
| x\yz | 00 | 01 | 11 | 10 |
|:--:|:--:|:--:|:--:|:--:|
| x' | 1 | 0 | 1 | 0 |
| x | 1 | 0 | 1 | 0 |

→ Se agrupan columnas 00 y 11 (ambas filas) → **F = y'z' + yz**

**Ejemplo con XOR/XNOR**: Si el mapa forma un patrón tipo "tablero de ajedrez", no siempre se obtiene la mínima expresión directamente del mapa (típico de XOR/XNOR).

> [Resumen Completo](https://github.com/AxelAbarMe/Arquitectura/blob/main/General/Teor%C3%ADa/Semana05/Dise%C3%B1o%20logico.md) - Diseño Lógico y Mapas de Karnaugh.

---

## 5. Mapa Karnaugh de 4 Variables y Don't Care

### Reglas de Agrupación
- **Grupo de 2 celdas**: elimina 1 variable.
- **Grupo de 4 celdas**: elimina 2 variables.
- **Grupo de 8 celdas**: elimina 3 variables.
- **Grupo de 16 celdas** (mapa completo): eliminaría las 4 variables → F=1 siempre.
- Entre más grande el grupo, menos literales tiene el término resultante.

### Plantilla Base (4 variables) — Formato
| WX\YZ | Y'Z' (00) | Y'Z (01) | YZ (11) | YZ' (10) |
|:--:|:--:|:--:|:--:|:--:|
| **W'X' (00)** | 0 | 1 | 3 | 2 |
| **W'X (01)** | 4 | 5 | 7 | 6 |
| **WX (11)** | 12 | 13 | 15 | 14 |
| **WX' (10)** | 8 | 9 | 11 | 10 |

- Filas y columnas en **código Gray** (00,01,11,10); los bordes se envuelven (wraparound), por lo que la primera y última fila/columna también son adyacentes entre sí.
- Convención: las dos primeras variables del enunciado = filas; las dos últimas = columnas (la primera variable listada es el bit más significativo).

### Ejemplo Resuelto (4 variables)
F(x,y,z,w) = Σm(0,1,3,5,6,7,8,11,15)

| xy\zw | 00 | 01 | 11 | 10 |
|:--:|:--:|:--:|:--:|:--:|
| x'y' | 1 | 1 | 1 | 0 |
| x'y | 0 | 1 | 1 | 1 |
| xy | 0 | 0 | 1 | 0 |
| xy' | 1 | 0 | 1 | 0 |

**Agrupaciones**: columna zw=11 completa → **zw**; bloque {1,3,5,7} → **x'w**; par {0,8} → **y'z'w'**; par {6,7} → **x'yz**

→ **F = x'w + zw + y'z'w' + x'yz**

### El "No Importa" (Don't Care)
- **Definición**: celda cuya combinación nunca ocurre en la práctica, o cuyo resultado es indiferente; se marca con **X**.
- Una X **puede tratarse como 1** si ayuda a formar un grupo mayor, o **como 0** si no aporta beneficio; **nunca es obligatorio** usarla.

**Ejemplo donde SÍ sirve**: F(x,y,z)=Σm(0,2,6)+d(4)
- Sin usar el don't care: F = x'z' + yz' (2 términos)
- Usando el 4 como 1: celdas 0,2,4,6 comparten z=0 → **F = z'** (1 término, mucho más simple)

**Ejemplo donde NO sirve**: F(x,y,z)=Σm(0,3,5)+d(6)
- El 0,3,5 están aislados entre sí y el don't care (6) no es adyacente a ninguno → no reduce nada, se ignora (se deja en 0).

**Ejemplo con muchos Don't Care (4 variables)**: F(D,C,B,A)=Σm(3,7,9,12,13)+d(0,1,2,6,8,10,11,15)
- Gracias a la gran cantidad de X, se forman 2 grupos de 4 celdas: {3,7,11,15}→**BA** y {8,9,12,13}→**DB'**
→ **F = BA + DB'** (solo 2 términos de 2 literales, algo imposible sin los don't cares)

> [Resumen Completo](https://github.com/AxelAbarMe/Arquitectura/blob/main/General/Teor%C3%ADa/Semana06/MapaK4.md) - Mapa K de 4 variables y Mapas utilizando "Don't Care".

---

## 6. Compuertas NAND, Half Adder y Full Adder

### Ley de Morgan
- **(x · y)' = x' + y'**
- **(x + y)' = x' · y'**
- Nota física: dos inversores (NOT) en serie se cancelan lógicamente, solo agregan un pequeño retardo (letargo).

### Conversión de AND, OR y NOT a NAND
- **OR → NAND**: se agrega un NOT (doble NAND) en cada entrada; luego se combinan en una NAND final.
- **NOT → NAND**: se unen ambas entradas de la NAND con la misma variable (equivale a invertirla).
- **AND → NAND**: 2 formas — (1) aprovechar NOT sobrantes de una conversión OR cercana, o (2) agregar 2 NOT en la salida de la NAND.
- **Regla de optimización**: siempre revisar los NOT ya disponibles antes de agregar nuevas NAND, para minimizar la cantidad total de compuertas.

**Ejemplo: (A+B)·A' = F** (esquema simplificado)
```
A --NOT--\
          \
           NAND (=OR de A,B) --\
B --NOT--/                      \
                                  NAND --> F
A' (ya disponible) -------------/
```

### Circuito Combinacional
- La salida depende únicamente de las entradas actuales; no tiene memoria (puede cambiar sin depender del resultado anterior). El ejemplo más simple es el sumador.

### Half Adder
- Entradas: x, y. Salidas: S (suma), C (acarreo).

| x | y | S | C |
|:--:|:--:|:--:|:--:|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |

- **Mapa K de S**: patrón ajedrez (sin agrupación posible) → **S = x ⊕ y (XOR)**
- **Mapa K de C**: solo la celda 11=1 → **C = xy (AND)**

```
x ----|---|
      |   --XOR----S
y ----|---|
   |
   |------AND----C
```

### Full Adder
- Entradas: x, y, z (incluye acarreo previo). Salidas: S (suma), C (acarreo).

| x | y | z | S | C |
|:--:|:--:|:--:|:--:|:--:|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

- **Mapa K de S**: patrón ajedrez (sin agrupación) → **S = x ⊕ y ⊕ z** (dos XOR encadenadas)
- **Mapa K de C**: se ignora la agrupación óptima directa (xy+yz+xz) para reutilizar el XOR de S:
  - **C = xy + z(x ⊕ y)**

```
x ----+--XOR--+----------------XOR---> S
      |        \                /
y ----+         \___(S1)_______/
      |              |
      +--AND----(C1)--\
      |                 \
z ----+-----------------+--AND----(C2)--\
      |                                    OR---> C
      +-----------------------------------/
```

- **Diferencia clave**: en el Half Adder no hay acarreo de entrada (se usa solo para el bit menos significativo); en el Full Adder sí, y se encadenan para sumar números de varios bits.

> [Resumen Completo](https://github.com/AxelAbarMe/Arquitectura/blob/main/General/Teor%C3%ADa/Semana07/CompuertasNAND.md) - Compuertas NAND, half y full adder.

# Memoria

---

## 1. Conceptos base

| Concepto | Definición |
|---|---|
| **Bit** | Unidad mínima de información (0 o 1) |
| **Byte** | 8 bits. Cada posición de memoria almacena **1 byte** |
| **Dígito hexadecimal** | Representa **4 bits** (de `0` a `F`) |
| **Dirección** | Número que identifica cada byte de la memoria |
| **Segmento** | Bloque de memoria de hasta **64 KB** (en x86 de modo real) |
| **Dirección lógica** | Dirección dada por **segmento : desplazamiento** (16 bits cada uno) |
| **Dirección física** | Dirección real que llega a la memoria (**20 bits = 5 dígitos hexadecimales**) |

### Tabla hexadecimal

| Hex | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | A | B | C | D | E | F |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Decimal** | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 |
| **Binario** | 0000 | 0001 | 0010 | 0011 | 0100 | 0101 | 0110 | 0111 | 1000 | 1001 | 1010 | 1011 | 1100 | 1101 | 1110 | 1111 |

---

## 2. Dirección lógica → dirección física (x86)

* El **registro de segmento** (ej. el de código, `CS`) tiene **16 bits**, pero el bus de direcciones usa **20 bits**.
* Para obtener la dirección física inicial del segmento se **agrega un dígito hexadecimal extra (`0`)** a la derecha, es decir, se desplaza 4 bits (equivale a multiplicar por 16).
* **Dirección física = (segmento × 16) + desplazamiento**.

### Pasos

1. Tomar el segmento (4 dígitos hex).
2. Agregarle un `0` a la derecha (queda de 5 dígitos).
3. Sumar el desplazamiento (offset) en hexadecimal.

### Ejemplo

| Paso | Valor |
|---|---|
| Segmento lógico | `0CA2h` |
| Se agrega el dígito extra | `0CA20h` |
| En binario | `0000 1100 1010 0010 0000` |
| Desplazamiento (offset) | `002Bh` |
| **Dirección física** | `0CA20h + 002Bh =` **`0CA4Bh`** |

* **Por qué 64 KB por segmento:** el desplazamiento tiene 16 bits, por lo que va de `0000h` a `FFFFh` (65 536 direcciones).
* **Por qué 1 MB en total:** 20 bits permiten direcciones de `00000h` a `FFFFFh`.

---

## 3. Tamaños en hexadecimal

### 3.1 Potencias de 2

* `1 K = 2¹⁰ = 1024` bytes.
* Para convertir un tamaño: separar el factor y multiplicar por `2¹⁰`.

| Tamaño | Cálculo | Potencia | Hexadecimal |
|---|---|---|---|
| 1 KB | 2¹⁰ | 2¹⁰ | `0400h` |
| 8 KB | 2³ · 2¹⁰ | 2¹³ | `2000h` |
| 16 KB | 2⁴ · 2¹⁰ | 2¹⁴ | `4000h` |
| 64 KB | 2⁶ · 2¹⁰ | 2¹⁶ | `10000h` |
| 128 KB | 2⁷ · 2¹⁰ | 2¹⁷ | `20000h` |
| 1 MB | 2²⁰ | 2²⁰ | `100000h` |

> **Truco:** `1 KB = 400h`. Para otros tamaños se multiplica: `3 KB = 3 × 400h = C00h`, `6 KB = 1800h`, `10 KB = 2800h`.

### 3.2 Tabla "TAM HEX": 2ⁿ − 1

* **2ⁿ − 1 equivale a un número binario con `n` unos.** Agrupando de 4 en 4 desde la derecha se obtiene el hexadecimal.

| n | 2ⁿ − 1 | Binario | Hex |
|---|---|---|---|
| 1 | 1 | `1` | `1` |
| 2 | 3 | `11` | `3` |
| 3 | 7 | `111` | `7` |
| 4 | 15 | `1111` | `F` |
| 14 | 16 383 | `11 1111 1111 1111` | `3FFF` |
| 16 | 65 535 | `1111 1111 1111 1111` | `FFFF` |
| 17 | 131 071 | `1 1111 1111 1111 1111` | `1FFFF` |

### 3.3 Restar 1 a un número terminado en ceros

* Cada `0` se convierte en `F` y el primer dígito distinto de cero baja una unidad.

| Operación | Resultado |
|---|---|
| `2000h − 1` | `1FFFh` |
| `10000h − 1` | `FFFFh` |
| `6000h − 1` | `5FFFh` |

---

## 4. Suma hexadecimal

### Reglas

1. Alinear los números **a la derecha**, rellenando con ceros a la izquierda.
2. Sumar **columna por columna**, de derecha a izquierda, convirtiendo cada dígito a decimal.
3. Sumar también el **acarreo** de la columna anterior.
4. Si el resultado es **menor que 16**, se escribe su dígito hexadecimal. Si es **16 o más**, se **resta 16**, se escribe el resto y se **lleva 1** a la siguiente columna.
5. Si queda un acarreo final, se escribe a la izquierda.

### Ejemplo 1: `6FEFF + BCDE`

```
    6 F E F F
  + 0 B C D E     (BCDE se completa con un 0)
  -----------
    7 B B D D
```

| Columna (der. → izq.) | Dígitos | Acarreo entra | Suma decimal | Resultado | Acarreo sale |
|---|---|---|---|---|---|
| 1 | F + E | 0 | 15 + 14 = 29 | 29 − 16 = 13 = **D** | 1 |
| 2 | F + D | 1 | 15 + 13 + 1 = 29 | 13 = **D** | 1 |
| 3 | E + C | 1 | 14 + 12 + 1 = 27 | 11 = **B** | 1 |
| 4 | F + B | 1 | 15 + 11 + 1 = 27 | 11 = **B** | 1 |
| 5 | 6 + 0 | 1 | 6 + 0 + 1 = 7 | **7** | 0 |

**Resultado: `7BBDDh`**

### Ejemplo 2: `1EFBCDEF + 94ACDAD1`

```
    1 E F B C D E F
  + 9 4 A C D A D 1
  -----------------
    B 3 A 8 A 8 C 0
```

| Columna | Dígitos | Acarreo entra | Suma decimal | Resultado | Acarreo sale |
|---|---|---|---|---|---|
| 1 | F + 1 | 0 | 15 + 1 = 16 | 16 − 16 = **0** | 1 |
| 2 | E + D | 1 | 14 + 13 + 1 = 28 | 28 − 16 = 12 = **C** | 1 |
| 3 | D + A | 1 | 13 + 10 + 1 = 24 | 8 = **8** | 1 |
| 4 | C + D | 1 | 12 + 13 + 1 = 26 | 10 = **A** | 1 |
| 5 | B + C | 1 | 11 + 12 + 1 = 24 | 8 = **8** | 1 |
| 6 | F + A | 1 | 15 + 10 + 1 = 26 | 10 = **A** | 1 |
| 7 | E + 4 | 1 | 14 + 4 + 1 = 19 | 3 = **3** | 1 |
| 8 | 1 + 9 | 1 | 1 + 9 + 1 = 11 | 11 = **B** | 0 |

**Resultado: `B3A8A8C0h`**

> **Observación:** el segundo sumando se tomó como `94ACDAD1` (8 dígitos), que es el valor con el que coincide el resultado `B3A8A8C0` de las notas.

---

## 5. Fórmula del mapa de memoria

$$X_f - X_i + 1 = TAM$$

| Símbolo | Significado |
|---|---|
| `Xi` | Dirección **inicial** del bloque (hex) |
| `Xf` | Dirección **final** del bloque (hex), la **buscada** |
| `TAM` | **Tamaño total** del bloque en bytes (convertido a hex) |

### Despejes

| Quiero hallar | Fórmula |
|---|---|
| Dirección final | `Xf = TAM + Xi − 1` |
| Dirección inicial | `Xi = Xf − TAM + 1` |
| Tamaño | `TAM = Xf − Xi + 1` |

### Cómo leer el dibujo

* La **línea inferior** es la dirección **inicial** del primer bloque.
* Una **línea entre dos bloques** es la **inicial del bloque de arriba**, que equivale a `Xf` del bloque de abajo `+ 1`.
* La **línea superior** es la dirección **final** (`Xf`) del último bloque.
* Las letras `x, y, z…` son **direcciones** desconocidas; `w, t` son **tamaños** desconocidos.
* Las direcciones **crecen hacia arriba**.

---

## 6. Procedimientos paso a paso

### A. Hallar la dirección de arriba conociendo el inicio y el tamaño del bloque

1. Convertir el tamaño a bytes y luego a hexadecimal.
2. Calcular `Xf = TAM + Xi − 1`.
3. La línea superior del bloque es `Xf + 1`.

### B. Hallar el inicio de un bloque desde el tope del mapa (hacia atrás)

1. Tomar `Xf` del bloque (si viene de una línea, `Xf = línea − 1`).
2. Convertir el tamaño a hexadecimal.
3. Calcular `Xi = Xf − TAM + 1`.

### C. Hallar un tamaño desconocido

1. Identificar `Xi` y `Xf` (recordar `Xf = línea superior − 1`).
2. Calcular `TAM = Xf − Xi + 1`.
3. Convertir de hexadecimal a KB: dividir entre `400h` o pasar a bytes y dividir entre 1024.

### D. Verificación final

* `Xf(tope) − Xi(base) + 1` debe ser igual a la **suma de todos los bloques**.

---

## 7. Ejemplo de las notas: segmento de 128 KB

> Un segmento de memoria de tamaño **X = 128 KB** empieza en **00000h**. Encuentre las direcciones **ZZZZZ** (tope) y **yyyyy** (techo del bloque azul). El bloque azul de **16 KB** empieza en **02000h**.

### Paso 1: hallar ZZZZZ (tope del segmento)

| Dato | Valor |
|---|---|
| `Xi` | `00000h` |
| `TAM` | `128 KB = 2⁷ · 2¹⁰ = 2¹⁷` |

```
Xf − Xi + 1 = TAM
Xf − 0 + 1 = 2¹⁷
Xf = 2¹⁷ − 1
```

* `2¹⁷ − 1` son 17 unos: `0001 1111 1111 1111 1111`.
* **ZZZZZ = `1FFFFh`**

### Paso 2: hallar yyyyy (techo del bloque azul de 16 KB)

| Dato | Valor |
|---|---|
| `Xi` (bloque azul) | `02000h` |
| `TAM` | `16 KB = 2⁴ · 2¹⁰ = 2¹⁴` |

1. Fin del bloque: `α − 02000 + 1 = 2¹⁴` → `α = 2¹⁴ − 1 + 02000`.
2. `2¹⁴ − 1 = 0000 0011 1111 1111 1111 = 03FFFh`.
3. Suma hexadecimal:

```
    0 3 F F F
  + 0 2 0 0 0
  -----------
    0 5 F F F   = α
```

4. La línea superior del bloque es `α + 1`:

```
α + 1 = 05FFFh + 1 = 06000h
```

**yyyyy = `06000h`**

### Resumen

| Variable | Representa | Valor |
|---|---|---|
| `ZZZZZ` | Dirección final del segmento (tope de 128 KB) | **`1FFFFh`** |
| `yyyyy` | Dirección inmediatamente superior al bloque azul | **`06000h`** |

> **Observación:** la cota de **1 KB** del dibujo es una referencia de escala y no interviene en el cálculo.

### Caso base: segmento de 64 KB desde 0

```
Xf − 0 + 1 = 64 K = 2⁶ · 2¹⁰ = 2¹⁶
Xf = 2¹⁶ − 1 = 0FFFFh
```

---

## 8. Ejemplos adicionales

### Ejemplo A: tres bloques desde `0000h` (RAM 8 KB, ROM 32 KB, Libre 16 KB)

| Paso | Cálculo | Resultado |
|---|---|---|
| `x` (fin de RAM + 1) | `Xf = 2000h + 0000h − 1 = 1FFFh` | **`x = 2000h`** |
| `y` (fin de ROM + 1) | `Xf = 8000h + 2000h − 1 = 9FFFh` | **`y = A000h`** |
| `z` (tope de Libre) | `Xf = 4000h + A000h − 1 = DFFFh` | **`z = DFFFh`** |

* **Verificación:** `DFFFh − 0000h + 1 = E000h = 56 KB` (8 + 32 + 16) ✔️

### Ejemplo B: hacia atrás desde el tope `FFFFh` (E/S 32 KB, ROM 16 KB, Libre 12 KB, RAM 4 KB)

| Bloque | `Xf` | Cálculo `Xi = Xf − TAM + 1` | Resultado |
|---|---|---|---|
| E/S (32 KB = `8000h`) | `FFFFh` | `FFFFh − 8000h + 1` | **`u = 8000h`** |
| ROM (16 KB = `4000h`) | `7FFFh` | `7FFFh − 4000h + 1` | **`z = 4000h`** |
| Libre (12 KB = `3000h`) | `3FFFh` | `3FFFh − 3000h + 1` | **`y = 1000h`** |
| RAM (4 KB = `1000h`) | `0FFFh` | `0FFFh − 1000h + 1` | **`x = 0000h`** |

### Ejemplo C: tamaño desconocido (Bloque 1 de 8 KB, Bloque 2 = `w`, Bloque 3 de 8 KB, tope `7FFFh`)

1. `x` (inicio del Bloque 3): `Xi = 7FFFh − 2000h + 1 = 6000h`.
2. `w`: `Xi = 2000h`, `Xf = 6000h − 1 = 5FFFh`.
3. `TAM = 5FFFh − 2000h + 1 = 4000h = 16384 bytes = 16 KB`.

**`x = 6000h`, `w = 16 KB`**

### Ejemplo D: 128 KB con hueco (RAM 12 KB, Video 20 KB, Libre `w`, ROM 64 KB, tope `1FFFFh`)

| Paso | Cálculo | Resultado |
|---|---|---|
| `x` | `03000h + 00000h − 1 = 02FFFh`, más 1 | **`x = 03000h`** |
| `y` | `05000h + 03000h − 1 = 07FFFh`, más 1 | **`y = 08000h`** |
| `z` (ROM desde el tope) | `1FFFFh − 10000h + 1` | **`z = 10000h`** |
| `w` | `0FFFFh − 08000h + 1 = 08000h` | **`w = 32 KB`** |

---

# Assembly

---

## 1. Arquitectura Von Neumann

* Modelo en el que el computador usa **una sola memoria** para almacenar **instrucciones y datos**, conectada a la CPU mediante **buses**.

| Componente | Función |
|---|---|
| **CPU** | Ejecuta las instrucciones. Contiene la **unidad de control**, la **ALU** y los **registros** |
| **Unidad de control (UC)** | Busca, interpreta y coordina la ejecución de cada instrucción |
| **ALU** | Realiza operaciones aritméticas y lógicas |
| **Registros** | Memorias **muy pequeñas y rápidas** dentro de la CPU |
| **Memoria** | Guarda programa y datos (organizada en segmentos) |
| **Buses** | **Direcciones** (qué posición), **datos** (qué valor) y **control** (qué operación) |
| **Entrada / Salida** | Comunicación con dispositivos |

### Ciclo de instrucción

1. **Búsqueda (fetch):** la CPU toma la instrucción de memoria (la dirección la dan `CS:IP`).
2. **Decodificación:** la unidad de control interpreta la instrucción.
3. **Ejecución:** la ALU opera sobre los registros o la memoria.
4. **Almacenamiento:** se guarda el resultado.

* **Cola de instrucciones (prefetch):** la CPU precarga instrucciones en una cola de **4 o 6 bytes** para trabajar más rápido.
* **Para qué sirve en Assembly:** programar en ensamblador es **manipular directamente estos registros y posiciones de memoria**, por lo que se debe saber dónde está cada dato.

---

## 2. Registros del procesador

### 2.1 Registros de propósito general (16 bits)

* Cada uno se puede usar completo (**16 bits**) o dividido en dos mitades de **8 bits** (alta `H` y baja `L`).

| Registro | Mitades | Nombre | Uso principal |
|---|---|---|---|
| **AX** | `AH` (8) · `AL` (8) | **Acumulador** | Operaciones aritméticas, resultados y servicios del DOS |
| **BX** | `BH` (8) · `BL` (8) | **Base / índice** | Direccionamiento de datos en el segmento de datos |
| **CX** | `CH` (8) · `CL` (8) | **Contador** | **Contador de loop** (ciclos) |
| **DX** | `DH` (8) · `DL` (8) | **Datos** | Datos y operaciones de E/S |

```
AX (16 bits)
┌──────────┬──────────┐
│    AH    │    AL    │
│ (8 bits) │ (8 bits) │
└──────────┴──────────┘
```

* `AH` = parte **alta**, `AL` = parte **baja**, `AX` = las dos juntas. Lo mismo aplica a `BX`, `CX` y `DX`.

### 2.2 Registros de segmento

| Registro | Significado | Uso |
|---|---|---|
| **CS** | **C**ode **S**egment | Segmento del **código** (programa) |
| **DS** | **D**ata **S**egment | Segmento de los **datos** |
| **SS** | **S**tack **S**egment | Segmento de la **pila** |
| **ES** | **E**xtra **S**egment | Segmento **extra** de datos |

* Una letra identifica el tipo (**C**ode, **D**ata, **S**tack, **E**xtra) y `S` significa **Segment**.
* **Mínimo para un programa:** `CS`, `DS` y `SS` (`ES` es adicional).
* Cada uno guarda el segmento (16 bits) que, con el **desplazamiento**, forma la dirección física (`segmento × 16 + offset`, visto en Memoria).

### 2.3 Otros registros

| Registro | Función |
|---|---|
| **IP** | Apunta a la **siguiente instrucción** (junto con `CS`) |
| **SP / BP** | Apuntan a la **pila** (junto con `SS`) |
| **SI / DI** | Índices para recorrer datos |
| **Banderas (flags)** | Indican el resultado de operaciones (ej. **bandera de signo**, cero, acarreo) |

### 2.4 Cómo se combinan con la memoria

| Par | Qué direcciona |
|---|---|
| `CS : IP` | Próxima instrucción a ejecutar |
| `DS : BX` | Dato en el segmento de datos |
| `SS : SP` | Tope de la pila |

---

## 3. Formatos de programa: .COM y .EXE

| Aspecto | `.COM` | `.EXE` |
|---|---|---|
| **Memoria** | **Un solo segmento de 64 KB** (código, datos y pila juntos) | **Varios segmentos** (pila, datos, código) |
| **Inicio** | `ORG 100h` | Procedimiento principal `PROC FAR` |
| **Estructura** | Simple | Más completa, con `ASSUME` |

* **`ORG 100h`:** el programa empieza en la dirección `100h` (256 bytes, reservados por el sistema). En binario: `0001 0000 0000 − 1 = 0000 1111 1111 = 0FFh`.
* **Estructura `.EXE`:** `BEGIN PROC FAR`, `ASSUME` (asocia cada segmento con su registro), segmentos de pila, datos y código.

### Dos formas de escribir (.EXE)

| Formato | Característica |
|---|---|
| **Tradicional** | Define explícitamente cada segmento (`SEGMENT ... ENDS`) |
| **Moderno** | Usa directivas simplificadas (`.MODEL`, `.STACK`, `.DATA`, `.CODE`) |

---

## 4. Formato tradicional

```asm
STACKSG SEGMENT PARA STACK 'Stack'
        DW 32 DUP(?)                 ; 32 palabras sin valor inicial
STACKSG ENDS

DATASG  SEGMENT PARA 'Data'
        ; DB ?  -> byte sin valor
        ; DW ?  -> palabra sin valor (o con otro valor)
DATASG  ENDS

CODESG  SEGMENT PARA 'Code'
BEGIN   PROC FAR
        ASSUME SS:STACKSG, DS:DATASG, CS:CODESG
        MOV  AX, DATASG              ; obtiene la dirección del segmento de datos
        MOV  DS, AX                  ; la almacena en DS
        ; ... instrucciones del programa ...
        MOV  AX, 4C00h               ; petición de terminar el programa
        INT  21h                     ; salida al DOS
BEGIN   ENDP
CODESG  ENDS
        END  BEGIN
```

| Línea | Función |
|---|---|
| `STACKSG SEGMENT PARA STACK 'Stack'` | Define el **segmento de pila** |
| `DATASG SEGMENT PARA 'Data'` | Define el **segmento de datos** |
| `CODESG SEGMENT PARA 'Code'` | Define el **segmento de código** |
| `DB` | Define un **byte** (`?` = sin valor) |
| `DW` | Define una **palabra** de 16 bits (`?` = sin valor, o un valor inicial) |
| `DW 32 DUP(?)` | Reserva **32 palabras** sin valor (`DUP` = duplicar) |
| `BEGIN PROC FAR` | Inicio del procedimiento principal |
| `ASSUME` | Indica qué segmento usa cada registro (`SS`, `DS`, `CS`) |
| `MOV AX, DATASG` / `MOV DS, AX` | **Inicializa `DS`**: no se puede cargar directamente, se pasa por `AX` |
| `MOV AX, 4C00h` / `INT 21h` | **Termina el programa** y regresa al DOS |
| `BEGIN ENDP` / `END BEGIN` | Fin del procedimiento y del programa |

---

## 5. Formato moderno

```asm
.MODEL SMALL
.STACK 64
.DATA
    FLDA DW 250
    FLDB DW 125
    FLDC DW ?
.CODE
BEGIN PROC FAR
    MOV  AX, @data        ; dirección del segmento de datos
    MOV  DS, AX           ; inicializa DS
    MOV  AX, FLDA         ; AX = 250
    ADD  AX, FLDB         ; AX = AX + 125 = 375
    MOV  FLDC, AX         ; guarda el resultado en FLDC
    MOV  AX, 4C00h
    INT  21h
BEGIN ENDP
END BEGIN
```

| Directiva | Función |
|---|---|
| `.MODEL SMALL` | Modelo de memoria pequeño (un segmento de código y uno de datos) |
| `.STACK 64` | Reserva **64 bytes** para la pila |
| `.DATA` | Inicio de los datos |
| `.CODE` | Inicio del código |
| `@data` | Dirección del segmento de datos |
| `FLDA DW 250` | Variable de 16 bits con el valor 250 |
| `FLDC DW ?` | Variable de 16 bits **sin valor** (guarda el resultado) |

* **Resultado:** `FLDC = 250 + 125 = 375`.

---

## 6. Instrucciones básicas

### 6.1 Transferencia y aritmética

| Instrucción | Función | Ejemplo |
|---|---|---|
| **MOV** | **Copiar** el segundo operando en el primero (movimiento de datos) | `MOV BX, AX` |
| **ADD** | **Suma** | `ADD AX, FLDB` |
| **SUB** | **Resta** | `SUB AH, 254` |
| **INT** | **Interrupción** (llama servicios del sistema) | `INT 21h` |

### 6.2 MOV con valor inmediato

* Un **valor inmediato** es un número escrito directamente en la instrucción.

```asm
MOV AX, 250        ; decimal (por defecto)
MOV AX, 0FAh       ; hexadecimal (0FAh = 250)
SUB AH, 254        ; resta un inmediato a la parte alta de AX
MOV BX, AX         ; copia el contenido de AX en BX
```

### 6.3 Sistemas numéricos (sufijos)

| Sufijo | Sistema | Ejemplo |
|---|---|---|
| **b** | Binario | `1111b` |
| **h** | Hexadecimal | `0FAh` |
| **d** | Decimal (**por defecto**) | `250` |

* Un número hexadecimal que **empieza con letra** lleva un `0` delante (`0FAh`).

### 6.4 Instrucciones lógicas

| Instrucción | Operación |
|---|---|
| **NOT** | Invierte los bits |
| **AND** | Y lógico bit a bit |
| **OR** | O lógico bit a bit |
| **XOR** | O exclusivo |
| **TEST** | Como `AND`, pero **solo modifica banderas** |
| **NEG** | Cambia el signo (complemento a dos); relacionada con la **bandera de signo** |

### 6.5 Desplazamientos y giros

| Grupo | Instrucciones | Descripción |
|---|---|---|
| **Desplazamiento** | `SHL`, `SAL` | Mueven los bits a la **izquierda** |
| | `SHR`, `SAR` | Mueven los bits a la **derecha** (`SAR` conserva el **signo**) |
| **Giros** | `ROL`, `ROR` | Giran los bits a la izquierda / derecha |
| | `RCL`, `RCR` | Giran **a través del acarreo** (izquierda / derecha) |

### 6.6 Ciclos

* Los ciclos se comportan como **DO-WHILE**: el cuerpo se ejecuta **al menos una vez** y la condición se evalúa al final.
* El registro **`CX` es el contador de loop**: en cada vuelta disminuye y el ciclo termina al llegar a cero.

---

## 7. Resumen general

* La **dirección física** se obtiene de la dirección lógica agregando un dígito hexadecimal extra (`0`) al segmento y sumando el desplazamiento: `segmento × 16 + offset`.
* Un segmento tiene **64 KB**; 20 bits direccionan **1 MB** (`00000h`–`FFFFFh`).
* **2ⁿ − 1** equivale a `n` unos en binario; agrupando de 4 en 4 se obtiene el hexadecimal (`2¹⁶ − 1 = FFFFh`, `2¹⁷ − 1 = 1FFFFh`).
* La **suma hexadecimal** se hace columna por columna: si la suma es 16 o más, se resta 16 y se lleva 1.
* La fórmula **`Xf − Xi + 1 = TAM`** permite hallar direcciones finales, iniciales o tamaños; la línea entre dos bloques es `Xf + 1` del bloque inferior.
* Se verifica siempre que `Xf(tope) − Xi(base) + 1` sea igual a la suma de los bloques.
* La arquitectura **Von Neumann** usa una memoria única para instrucciones y datos, comunicada con la CPU (UC, ALU y registros) mediante buses.
* Los registros **AX, BX, CX, DX** (16 bits) se dividen en mitades `H` y `L` de 8 bits; **CS, DS, SS, ES** guardan los segmentos de código, datos, pila y extra.
* Un programa en ensamblador puede ser **.COM** (un segmento de 64 KB, `ORG 100h`) o **.EXE** (varios segmentos, `PROC FAR`, `ASSUME`), en formato **tradicional** o **moderno**.
* Las instrucciones básicas son **MOV, ADD, SUB, INT**, las lógicas **NOT, AND, OR, XOR, TEST, NEG**, los desplazamientos **SHL, SAL, SHR, SAR** y los giros **ROL, ROR, RCL, RCR**.
* Todo programa **.EXE** termina con `MOV AX, 4C00h` e `INT 21h`.
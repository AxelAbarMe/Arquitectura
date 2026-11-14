# Guía de resolución del Examen Final: Mapa de memoria, Pila con mapa inicial y Desplazamientos/LOOP

---

## 0. Qué se evalúa y qué cambia en cada grupo

| Ejercicio | Tema | Qué hay que entregar |
|---|---|---|
| **1** | Segmento de memoria (fórmula `Xf − Xi + 1 = TAM`) | Direcciones **`ZZZZZ`** (tope del segmento) y **`yyyyy`** (techo del bloque azul) |
| **2** | Pila con **mapa inicial** (`PUSH`, `POP`, `MOV reg,SS:[dir]`) | `BX`, `AX`, `SS:[dirección]` y `SP` finales |
| **3** | `SHL`, `SAR`, `RCR`, `RCL`, `NOT`, `LOOP`, `JNC/JC`, `INC`, `ADD`, `SAR`, `XCHG`, `AND/OR/XOR`, `NEG` | Valores intermedios (`AH/BH` o `AL/BL`) y finales (`AX`, `BX`) |

| Dato que cambia | Grupo **A** | Grupo **B** | Grupo **C** |
|---|---|---|---|
| Ej. 1: tamaño del segmento `X` | **512 KB** | **256 KB** | **128 KB** |
| Ej. 1: bloque azul (inicia en `02000h`) | **256 KB** | **8 KB** | **16 KB** |
| Ej. 2: `SP` inicial | `2000H` | `2001H` | `2000H` |
| Ej. 2: celda pedida | `SS:[1FFDH]` | `SS:[1FFEH]` | `SS:[1FFDH]` |
| Ej. 3: `BX` inicial | `0FBC1H` | `0DBCEH` | `0DBCEH` |
| Ej. 3: operación final | `OR BH,10111100B` + `NEG AX` | `AND AH,10111111B` + `NEG BX` | `XOR BL,0AAH` + `NEG AX` |

---

# PARTE 1. HERRAMIENTAS MATEMÁTICAS (se usan en los 3 ejercicios)

## 1.1 Tabla hexadecimal ↔ binario

| Hex | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | A | B | C | D | E | F |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Dec** | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 |
| **Bin** | 0000 | 0001 | 0010 | 0011 | 0100 | 0101 | 0110 | 0111 | 1000 | 1001 | 1010 | 1011 | 1100 | 1101 | 1110 | 1111 |

* **Hex → binario:** cada dígito se expande a **4 bits**. Ejemplo: `DBCEh` = `1101 1011 1100 1110`.
* **Binario → hex:** se agrupa de **4 en 4 desde la derecha**. Ejemplo: `1110 0110 1100 1111` = `E6CFh`.
* Un registro de 16 bits = **4 dígitos hex = 16 bits**; un registro de 8 bits (`AH`, `BL`…) = **2 dígitos hex = 8 bits**.

## 1.2 Suma hexadecimal (columna por columna)

1. Alinear a la derecha (rellenar con ceros a la izquierda).
2. De derecha a izquierda: convertir cada dígito a decimal y sumar también el **acarreo** de la columna anterior.
3. Si la suma es **< 16** se escribe su dígito hex. Si es **≥ 16**, se **resta 16**, se escribe el resto y se **lleva 1**.
4. Un acarreo final se escribe a la izquierda.

**Sumar 1 (`INC`, `ADD x,1`)**: el último dígito sube 1; si era `F` pasa a `0` y se lleva 1.
* `DBCEh + 1 = DBCFh` (E+1 = F, sin acarreo).
* `41FFFh + 1`: `F+1 = 16 → 0` (lleva 1), `F+1 → 0`, `F+1 → 0`, `1+1 = 2` → **`42000h`**.

## 1.3 Potencias de 2 en hexadecimal

* `1 K = 2¹⁰ = 1024` bytes; **`1 KB = 400h`**.
* **Regla rápida:** `2^(4q+r) = (2^r) seguido de q ceros en hexadecimal`.

| Potencia | Descomposición | Hex |
|---|---|---|
| `2¹³` (8 KB) | 13 = 4·3 + 1 → `2¹` y 3 ceros | `2000h` |
| `2¹⁴` (16 KB) | 14 = 4·3 + 2 → `2²` y 3 ceros | `4000h` |
| `2¹⁷` (128 KB) | 17 = 4·4 + 1 → `2¹` y 4 ceros | `20000h` |
| `2¹⁸` (256 KB) | 18 = 4·4 + 2 → `2²` y 4 ceros | `40000h` |
| `2¹⁹` (512 KB) | 19 = 4·4 + 3 → `2³` y 4 ceros | `80000h` |

## 1.4 Número `2ⁿ − 1`

* `2ⁿ − 1` es un binario de **`n` unos**; se agrupa de 4 en 4 desde la derecha.

| n | Binario | Hex |
|---|---|---|
| 13 | `1 1111 1111 1111` | `1FFFh` |
| 14 | `11 1111 1111 1111` | `3FFFh` |
| 17 | `1 1111 1111 1111 1111` | `1FFFFh` |
| 18 | `11 1111 1111 1111 1111` | `3FFFFh` |
| 19 | `111 1111 1111 1111 1111` | `7FFFFh` |

## 1.5 Complemento a dos y `NEG`

* `NEG x` = **complemento a dos** = `0 − x`. En 16 bits: **`10000h − x`**. En 8 bits: `100h − x`.
* **Método:** (1) invertir todos los bits (complemento a 1), (2) sumar 1.
* **Comprobación:** `x + NEG(x) = 10000h`.

Ejemplo: `NEG AX` con `AX = CF98h`:
```
CF98h = 1100 1111 1001 1000
invertir → 0011 0000 0110 0111 = 3067h
  + 1    → 0011 0000 0110 1000 = 3068h     (CF98h + 3068h = 10000h ✔)
```

## 1.6 Operaciones lógicas bit a bit

| Operación | Regla | Resultado queda en |
|---|---|---|
| `AND d,o` | 1 solo si **ambos** son 1 | `d` |
| `OR d,o` | 1 si **al menos uno** es 1 | `d` |
| `XOR d,o` | 1 si los bits son **distintos** | `d` |
| `NOT d` | Invierte cada bit | `d` |

* El sufijo **`B`** indica binario (`10111111B`); **`H`** hexadecimal; sin sufijo es **decimal**.
* Hexadecimal que **empieza con letra** lleva `0` delante (`0AAH`, `0DBCEH`).

---

# PARTE 2. EJERCICIO 1: SEGMENTO DE MEMORIA

## 2.1 Fórmula y lectura del dibujo

$$X_f - X_i + 1 = TAM \qquad\Longrightarrow\qquad X_f = TAM + X_i - 1$$

| Símbolo | Significado |
|---|---|
| `Xi` | Dirección **inicial** del bloque |
| `Xf` | Dirección **final** (último byte) del bloque |
| `TAM` | Tamaño en bytes (en hexadecimal) |

* Las direcciones **crecen hacia arriba**.
* **`ZZZZZ`** (línea superior del segmento) = `Xf` del segmento completo.
* **`yyyyy`** (techo del bloque azul) = `Xf` del bloque azul **+ 1** (una línea entre bloques es la inicial del bloque de arriba).
* La cota de **1 KB** del dibujo es solo una referencia de escala y **no interviene** en el cálculo.

## 2.2 Procedimiento paso a paso

**Para `ZZZZZ`:**
1. Convertir `X` a potencia de 2: `X = k · 2¹⁰`.
2. Como `Xi = 00000h`: `Xf − 0 + 1 = 2ⁿ` → `Xf = 2ⁿ − 1`.
3. `2ⁿ − 1` = `n` unos → agrupar de 4 en 4 → hexadecimal.

**Para `yyyyy`:**
1. Convertir el tamaño del bloque azul a hexadecimal (regla 1.3).
2. Calcular `Xf(azul) = TAM + 02000h − 1` (primero `TAM − 1`, luego sumar `02000h`).
3. `yyyyy = Xf(azul) + 1`.

## 2.3 Grupo C: `X = 128 KB`, bloque azul `16 KB`

**`ZZZZZ`:**
```
128 KB = 2⁷ · 2¹⁰ = 2¹⁷
Xf = 2¹⁷ − 1 = 17 unos = 0001 1111 1111 1111 1111 = 1FFFFh
```
**`yyyyy`:**
```
16 KB = 2⁴ · 2¹⁰ = 2¹⁴ = 4000h
TAM − 1 = 2¹⁴ − 1 = 03FFFh
Xf(azul) = 03FFFh + 02000h:
    0 3 F F F
  + 0 2 0 0 0
  -----------
    0 5 F F F        (F+0=F, F+0=F, F+0=F, 3+2=5)
yyyyy = 05FFFh + 1 = 06000h
```

| Variable | Valor |
|---|---|
| **ZZZZZ** | **`1FFFFh`** |
| **yyyyy** | **`06000h`** |

Verificación: `06000h − 02000h = 4000h = 16 KB` ✔.

## 2.4 Grupo B: `X = 256 KB`, bloque azul `8 KB`

**`ZZZZZ`:**
```
256 KB = 2⁸ · 2¹⁰ = 2¹⁸
Xf = 2¹⁸ − 1 = 18 unos = 11 1111 1111 1111 1111 = 3FFFFh
```
**`yyyyy`:**
```
8 KB = 2³ · 2¹⁰ = 2¹³ = 2000h
TAM − 1 = 2¹³ − 1 = 01FFFh
Xf(azul) = 01FFFh + 02000h:
    0 1 F F F
  + 0 2 0 0 0
  -----------
    0 3 F F F
yyyyy = 03FFFh + 1 = 04000h
```

| Variable | Valor |
|---|---|
| **ZZZZZ** | **`3FFFFh`** |
| **yyyyy** | **`04000h`** |

Verificación: `04000h − 02000h = 2000h = 8 KB` ✔; `3FFFFh + 1 = 40000h = 256 KB` ✔.

## 2.5 Grupo A: `X = 512 KB`, bloque azul `256 KB`

**`ZZZZZ`:**
```
512 KB = 2⁹ · 2¹⁰ = 2¹⁹
Xf = 2¹⁹ − 1 = 19 unos = 111 1111 1111 1111 1111 = 7FFFFh
```
**`yyyyy`:**
```
256 KB = 2⁸ · 2¹⁰ = 2¹⁸ = 40000h
TAM − 1 = 2¹⁸ − 1 = 3FFFFh
Xf(azul) = 3FFFFh + 02000h:
    3 F F F F
  + 0 2 0 0 0
  -----------
    4 1 F F F        (F+0=F, F+0=F, F+0=F, F+2=17→1 lleva 1, 3+0+1=4)
yyyyy = 41FFFh + 1 = 42000h      (F+1=16→0 lleva 1, en cadena)
```

| Variable | Valor |
|---|---|
| **ZZZZZ** | **`7FFFFh`** |
| **yyyyy** | **`42000h`** |

Verificación: `42000h − 02000h = 40000h = 256 KB` ✔; `7FFFFh + 1 = 80000h = 512 KB` ✔.

## 2.6 Resumen del Ejercicio 1

| Grupo | `X` | Bloque azul | `ZZZZZ` | `yyyyy` |
|---|---|---|---|---|
| **A** | 512 KB | 256 KB | **`7FFFFh`** | **`42000h`** |
| **B** | 256 KB | 8 KB | **`3FFFFh`** | **`04000h`** |
| **C** | 128 KB | 16 KB | **`1FFFFh`** | **`06000h`** |

## 2.7 Otros formatos del mismo tipo (resueltos)

**Despejes:** `Xf = TAM + Xi − 1` · `Xi = Xf − TAM + 1` · `TAM = Xf − Xi + 1`.

**Ejemplo A: tres bloques desde `0000h` (RAM 8 KB, ROM 32 KB, Libre 16 KB)**

| Paso | Cálculo | Resultado |
|---|---|---|
| `x` (techo de RAM) | `Xf = 2000h + 0000h − 1 = 1FFFh` → +1 | **`x = 2000h`** |
| `y` (techo de ROM) | `Xf = 8000h + 2000h − 1 = 9FFFh` → +1 | **`y = A000h`** |
| `z` (tope de Libre) | `Xf = 4000h + A000h − 1 = DFFFh` | **`z = DFFFh`** |

Verificación: `DFFFh − 0000h + 1 = E000h = 56 KB` (8 + 32 + 16) ✔.

**Ejemplo B: hacia atrás desde el tope `FFFFh`** (E/S 32 KB, ROM 16 KB, Libre 12 KB, RAM 4 KB)

| Bloque | `Xf` | `Xi = Xf − TAM + 1` | Resultado |
|---|---|---|---|
| E/S (`8000h`) | `FFFFh` | `FFFFh − 8000h + 1` | **`u = 8000h`** |
| ROM (`4000h`) | `7FFFh` | `7FFFh − 4000h + 1` | **`z = 4000h`** |
| Libre (`3000h`) | `3FFFh` | `3FFFh − 3000h + 1` | **`y = 1000h`** |
| RAM (`1000h`) | `0FFFh` | `0FFFh − 1000h + 1` | **`x = 0000h`** |

**Ejemplo C: tamaño desconocido** (Bloque 1 = 8 KB, Bloque 2 = `w`, Bloque 3 = 8 KB, tope `7FFFh`)
1. Inicio del Bloque 3: `Xi = 7FFFh − 2000h + 1 = 6000h`.
2. Bloque 2: `Xi = 2000h`, `Xf = 6000h − 1 = 5FFFh`.
3. `TAM = 5FFFh − 2000h + 1 = 4000h = 16384 bytes = 16 KB`.

**Ejemplo D: 128 KB con hueco** (RAM 12 KB, Video 20 KB, Libre `w`, ROM 64 KB, tope `1FFFFh`)

| Paso | Cálculo | Resultado |
|---|---|---|
| `x` | `03000h + 00000h − 1 = 02FFFh` → +1 | **`x = 03000h`** |
| `y` | `05000h + 03000h − 1 = 07FFFh` → +1 | **`y = 08000h`** |
| `z` (ROM desde el tope) | `1FFFFh − 10000h + 1` | **`z = 10000h`** |
| `w` | `0FFFFh − 08000h + 1 = 08000h` | **`w = 32 KB`** |

**Conversión de tamaños:** `3 KB = 3·400h = C00h`, `6 KB = 1800h`, `10 KB = 2800h`, `12 KB = 3000h`, `20 KB = 5000h`. De hex a KB: dividir entre `400h`.

**Verificación final:** `Xf(tope) − Xi(base) + 1` = **suma de todos los bloques**.

*Extracto relacionado:* **dirección física = segmento × 16 + desplazamiento** (agregar un `0` a la derecha del segmento). Ej.: `CS = 1234h`, `IP = 0100h` → `12340h + 0100h = 12440h`.

---

# PARTE 3. EJERCICIO 2: PILA CON MAPA INICIAL

## 3.1 Conceptos

| Concepto | Explicación |
|---|---|
| **Pila (stack)** | Segmento donde la CPU guarda temporalmente información (ante una interrupción) y luego la recupera |
| **Celda** | **1 byte** con su propia dirección; las direcciones crecen en hexadecimal: `1FFD, 1FFE, 1FFF, 2000, 2001…` |
| **`SP`** | Registro de 16 bits con el **desplazamiento** dentro de `SS` (dirección lógica `SS:SP`) |
| **Inmediato** | `MOV SP,2000H` carga el valor directamente (hay que escribir `H`) |
| **`[ ]`** | Un lugar en la memoria |

### Reglas de `MOV reg,SS:[dir]`
1. Copia el **contenido** de la memoria al registro (la memoria **no cambia**).
2. Sin `SS:` se usaría el segmento de datos; aquí siempre aparece `SS:`.
3. **El tamaño lo decide el destino:**
   * Destino de **16 bits** (`AX, BX…`) → lee **2 bytes**: `[dir]` va a la parte **baja** y `[dir+1]` a la parte **alta**.
   * Destino de **8 bits** (`AL, BH…`) → lee **solo 1 byte** `[dir]` y la otra mitad del registro **no cambia**.

### `PUSH reg` (guardar)
1. **`SP ← SP − 2`**.
2. Parte **alta** del registro → `[SP+1]`; parte **baja** → `[SP]`.
3. El registro **no cambia** (es una copia). Lo que había en la memoria se **sobreescribe**.

### `POP reg` (recuperar)
1. Lee `[SP]` → parte **baja** y `[SP+1]` → parte **alta** del registro destino.
2. **`SP ← SP + 2`**.
3. La memoria **no se borra**: los bytes leídos quedan ahí.

**Fórmula:** `SP final = SP inicial − 2·(#PUSH) + 2·(#POP)`.

## 3.2 Cómo leer el mapa inicial del emulador

* Cada fila es `0100:DIR  VVVV`: `0100` = `SS`, `DIR` = dirección, `VVVV` = **palabra de 2 bytes**.
* Los **2 dígitos de la derecha** son el byte de **esa dirección** (bajo) y los **2 de la izquierda** el de la **dirección + 1** (alto).
* El mapa muestra filas en direcciones pares; los bytes de direcciones impares son los dígitos de la izquierda de la fila anterior.
* Todo lo que no aparece (fuera de la lista) vale `00`.

**Mapa inicial del examen (valores iniciales de la pila, iguales en los tres grupos):**

| Fila | Palabra | Byte en `DIR` (bajo) | Byte en `DIR+1` (alto) |
|---|---|---|---|
| `2010` | `0000` | `00` | `00` |
| `200E` | `0072` | `72` | `00` |
| `200C` | `3442` | `42` | `34` |
| `200A` | `9900` | `00` | `99` |
| `2008` | `0000` | `00` | `00` |
| `2006` | `0000` | `00` | `00` |
| `2004` | `0000` | `00` | `00` |
| `2002` | `0012` | `12` | `00` |
| `2000` | `1184` | `84` | `11` |
| `1FFE` | `0022` | `22` | `00` |
| `1FFC` | `4489` | `89` | `44` |
| `1FFA` | `2400` | `00` | `24` |
| `1FF8`…`1FF2` | `0000` | `00` | `00` |

**Mapa convertido a bytes (el que se usa para resolver):**

| Dirección | 1FFA | 1FFB | 1FFC | 1FFD | 1FFE | 1FFF | 2000 | 2001 | 2002 | 2003 |
|---|---|---|---|---|---|---|---|---|---|---|
| **Byte** | `00` | `24` | `89` | `44` | `22` | `00` | `84` | `11` | `12` | `00` |

*(Los valores de las instrucciones se leyeron de las imágenes del examen.)*

## 3.3 Método de resolución

1. Copiar el mapa de bytes en la hoja y marcar el `SP` inicial.
2. Por cada instrucción actualizar: registros, `SP` y los **bytes de memoria que cambian**.
3. En cada `MOV reg,SS:[dir]`: mirar si el destino es de 8 o 16 bits y leer los bytes **del estado actual** de la memoria (no del mapa inicial).
4. Un `MOV reg,valor` seguido de `POP reg` solo tiene efecto temporal: el `POP` sobreescribe el registro.
5. Respuesta final: registros, `SP` y el byte de la celda pedida (estado final).

## 3.4 Grupo C: resolución completa

```asm
MOV SP,2000H        MOV BX,SS:[1FFEH]
MOV AX,9209H        MOV BH,SS:[1FFDH]
PUSH AX             MOV AX,5C29H
MOV AX,3F89H        POP AX
POP AX              MOV AX,0BEA4H
MOV BX,SS:[2001H]   POP AX
PUSH BX             MOV AL,SS:[1FFDH]
MOV AX,1584H
PUSH AX
MOV CX,51F4H
POP CX
```

| # | Instrucción | Procedimiento | SP | Registros |
|---|---|---|---|---|
| 1 | `MOV SP,2000H` | Inmediato | `2000` | |
| 2 | `MOV AX,9209H` | | 2000 | `AX=9209` |
| 3 | `PUSH AX` | SP−2 = `1FFE`; `[1FFF]=92`, `[1FFE]=09` (antes `00`,`22`) | **`1FFE`** | |
| 4 | `MOV AX,3F89H` | | 1FFE | `AX=3F89` |
| 5 | `POP AX` | Lee `[1FFE]=09`(bajo), `[1FFF]=92`(alto); SP+2 | **`2000`** | `AX=9209` |
| 6 | `MOV BX,SS:[2001H]` | `[2001]=11`→BL, `[2002]=12`→BH | 2000 | `BX=1211` |
| 7 | `PUSH BX` | SP=`1FFE`; `[1FFF]=12`, `[1FFE]=11` | **`1FFE`** | |
| 8 | `MOV AX,1584H` | | 1FFE | `AX=1584` |
| 9 | `PUSH AX` | SP=`1FFC`; `[1FFD]=15`, `[1FFC]=84` (antes `44`,`89`) | **`1FFC`** | |
| 10 | `MOV CX,51F4H` | | 1FFC | `CX=51F4` |
| 11 | `POP CX` | Lee `[1FFC]=84`, `[1FFD]=15`; SP+2 | **`1FFE`** | `CX=1584` |
| 12 | `MOV BX,SS:[1FFEH]` | `[1FFE]=11`→BL, `[1FFF]=12`→BH | 1FFE | `BX=1211` |
| 13 | `MOV BH,SS:[1FFDH]` | Solo 1 byte: `[1FFD]=15`→BH | 1FFE | `BX=1511` |
| 14 | `MOV AX,5C29H` | | 1FFE | `AX=5C29` |
| 15 | `POP AX` | Lee `[1FFE]=11`, `[1FFF]=12`; SP+2 | **`2000`** | `AX=1211` |
| 16 | `MOV AX,0BEA4H` | | 2000 | `AX=BEA4` |
| 17 | `POP AX` | Lee `[2000]=84`, `[2001]=11` (del mapa inicial); SP+2 | **`2002`** | `AX=1184` |
| 18 | `MOV AL,SS:[1FFDH]` | Solo 1 byte: `[1FFD]=15`→AL | 2002 | `AX=1115` |

| BX | AX | SS:[1FFDH] | SP |
|---|---|---|---|
| **`1511h`** | **`1115h`** | **`15h`** | **`2002h`** |

## 3.5 Grupo B: resolución completa

```asm
MOV SP,2001H        MOV BX,SS:[1FFFH]
MOV AX,0FF19H       MOV BH,SS:[1FFEH]
PUSH AX             MOV AX,5132H
MOV AX,1F8CH        POP BX
POP AX              MOV AX,0BF34H
MOV BX,SS:[2000H]   POP AX
PUSH BX             MOV AL,SS:[1FFEH]
MOV AX,1584H
PUSH AX
MOV CX,54F4H
POP CX
```

| # | Instrucción | Procedimiento | SP | Registros |
|---|---|---|---|---|
| 1 | `MOV SP,2001H` | `SP` impar (las palabras empiezan en dirección impar) | `2001` | |
| 2 | `MOV AX,0FF19H` | | 2001 | `AX=FF19` |
| 3 | `PUSH AX` | SP=`1FFF`; `[2000]=FF`, `[1FFF]=19` (antes `84`,`00`) | **`1FFF`** | |
| 4 | `MOV AX,1F8CH` | | 1FFF | `AX=1F8C` |
| 5 | `POP AX` | Lee `[1FFF]=19`, `[2000]=FF`; SP+2 | **`2001`** | `AX=FF19` |
| 6 | `MOV BX,SS:[2000H]` | `[2000]=FF`→BL, `[2001]=11`→BH | 2001 | `BX=11FF` |
| 7 | `PUSH BX` | SP=`1FFF`; `[2000]=11`, `[1FFF]=FF` | **`1FFF`** | |
| 8 | `MOV AX,1584H` | | 1FFF | `AX=1584` |
| 9 | `PUSH AX` | SP=`1FFD`; `[1FFE]=15`, `[1FFD]=84` (antes `22`,`44`) | **`1FFD`** | |
| 10 | `MOV CX,54F4H` | | 1FFD | `CX=54F4` |
| 11 | `POP CX` | Lee `[1FFD]=84`, `[1FFE]=15`; SP+2 | **`1FFF`** | `CX=1584` |
| 12 | `MOV BX,SS:[1FFFH]` | `[1FFF]=FF`→BL, `[2000]=11`→BH | 1FFF | `BX=11FF` |
| 13 | `MOV BH,SS:[1FFEH]` | 1 byte: `[1FFE]=15`→BH | 1FFF | `BX=15FF` |
| 14 | `MOV AX,5132H` | | 1FFF | `AX=5132` |
| 15 | `POP BX` | Lee `[1FFF]=FF`, `[2000]=11`; SP+2 | **`2001`** | `BX=11FF` |
| 16 | `MOV AX,0BF34H` | | 2001 | `AX=BF34` |
| 17 | `POP AX` | Lee `[2001]=11`, `[2002]=12` (mapa inicial); SP+2 | **`2003`** | `AX=1211` |
| 18 | `MOV AL,SS:[1FFEH]` | 1 byte: `[1FFE]=15`→AL | 2003 | `AX=1215` |

| BX | AX | SS:[1FFEH] | SP |
|---|---|---|---|
| **`11FFh`** | **`1215h`** | **`15h`** | **`2003h`** |

## 3.6 Grupo A: resolución completa

```asm
MOV SP,2000H        MOV BX,SS:[1FFEH]
MOV AX,0FE29H       MOV BH,SS:[1FFDH]
PUSH AX             MOV AX,5A34H
MOV AX,9F2CH        POP BX
POP AX              MOV AX,0AF74H
MOV BX,SS:[2001H]   POP AX
PUSH BX             MOV AL,SS:[1FFAH]
MOV AX,2544H
PUSH AX
MOV CX,52F3H
POP CX
```

| # | Instrucción | Procedimiento | SP | Registros |
|---|---|---|---|---|
| 1 | `MOV SP,2000H` | | `2000` | |
| 2 | `MOV AX,0FE29H` | | 2000 | `AX=FE29` |
| 3 | `PUSH AX` | SP=`1FFE`; `[1FFF]=FE`, `[1FFE]=29` | **`1FFE`** | |
| 4 | `MOV AX,9F2CH` | | 1FFE | `AX=9F2C` |
| 5 | `POP AX` | Lee `[1FFE]=29`, `[1FFF]=FE`; SP+2 | **`2000`** | `AX=FE29` |
| 6 | `MOV BX,SS:[2001H]` | `[2001]=11`→BL, `[2002]=12`→BH | 2000 | `BX=1211` |
| 7 | `PUSH BX` | SP=`1FFE`; `[1FFF]=12`, `[1FFE]=11` | **`1FFE`** | |
| 8 | `MOV AX,2544H` | | 1FFE | `AX=2544` |
| 9 | `PUSH AX` | SP=`1FFC`; `[1FFD]=25`, `[1FFC]=44` (antes `44`,`89`) | **`1FFC`** | |
| 10 | `MOV CX,52F3H` | | 1FFC | `CX=52F3` |
| 11 | `POP CX` | Lee `[1FFC]=44`, `[1FFD]=25`; SP+2 | **`1FFE`** | `CX=2544` |
| 12 | `MOV BX,SS:[1FFEH]` | `[1FFE]=11`→BL, `[1FFF]=12`→BH | 1FFE | `BX=1211` |
| 13 | `MOV BH,SS:[1FFDH]` | 1 byte: `[1FFD]=25`→BH | 1FFE | `BX=2511` |
| 14 | `MOV AX,5A34H` | | 1FFE | `AX=5A34` |
| 15 | `POP BX` | Lee `[1FFE]=11`, `[1FFF]=12`; SP+2 | **`2000`** | `BX=1211` |
| 16 | `MOV AX,0AF74H` | | 2000 | `AX=AF74` |
| 17 | `POP AX` | Lee `[2000]=84`, `[2001]=11` (mapa inicial); SP+2 | **`2002`** | `AX=1184` |
| 18 | `MOV AL,SS:[1FFAH]` | 1 byte: `[1FFA]=00` (mapa inicial) → AL | 2002 | `AX=1100` |

| BX | AX | SS:[1FFDH] | SP |
|---|---|---|---|
| **`1211h`** | **`1100h`** | **`25h`** | **`2002h`** |

## 3.7 Resumen del Ejercicio 2

| Grupo | `BX` | `AX` | Celda pedida | `SP` |
|---|---|---|---|---|
| **A** | `1211h` | `1100h` | `SS:[1FFDH] = 25h` | `2002h` |
| **B** | `11FFh` | `1215h` | `SS:[1FFEH] = 15h` | `2003h` |
| **C** | `1511h` | `1115h` | `SS:[1FFDH] = 15h` | `2002h` |

*Si la celda se pidiera como palabra de 2 bytes (`[dir]` bajo, `[dir+1]` alto):* A = `[1FFD]=25`, `[1FFE]=11` → `1125h`; B = `[1FFE]=15`, `[1FFF]=FF` → `FF15h`; C = `[1FFD]=15`, `[1FFE]=11` → `1115h`.

## 3.8 Puntos donde se pierden puntos

* **`PUSH` baja antes de escribir:** nunca escribe en la celda donde apuntaba el `SP` antes del `PUSH`.
* **Alto/bajo:** `PUSH` guarda alto en `SP+1` y bajo en `SP`; `POP` y `MOV` de 16 bits lo leen igual (`dir` = bajo, `dir+1` = alto).
* **8 bits vs 16 bits:** `MOV BH,SS:[…]` y `MOV AL,SS:[…]` leen un solo byte y respetan la otra mitad.
* **`MOV reg,valor` + `POP reg`:** el `POP` borra el `MOV` anterior.
* **La memoria se lee del estado actual:** una celda puede haber sido sobreescrita por un `PUSH` previo; si nunca se tocó, vale lo del mapa inicial.
* **`SP` impar (Grupo B):** el `PUSH` escribe en `2000` y `1FFF` (cruza la frontera par/impar del mapa).

---

# PARTE 4. EJERCICIO 3: DESPLAZAMIENTOS, ROTACIONES, LOOP Y SALTOS

## 4.1 Desplazamientos y rotaciones

*(El dibujo de las flechas se entrega en el examen; lo que se evalúa es el procedimiento.)*

**Regla general:** `AX,2` significa repetir el movimiento de **1 bit dos veces, una tras otra, sobre el último valor**. Todos manipulan `CF`: queda el **último bit que salió**. En este examen todos los conteos son 1.

| Instrucción | Dirección | Bit que entra | Bit que sale a `CF` | ¿Usa el `CF` anterior? |
|---|---|---|---|---|
| **`SHL` / `SAL`** | Izquierda | `0` por el LSB | MSB | No |
| **`SHR`** | Derecha | `0` por el MSB | LSB | No |
| **`SAR`** | Derecha (aritmético) | **Se repite el bit de signo** (MSB) | LSB | No |
| **`RCL`** | Giro izquierda **con carry** | **`CF` anterior** por el LSB | MSB | **Sí** |
| **`RCR`** | Giro derecha **con carry** | **`CF` anterior** por el MSB | LSB | **Sí** |
| `ROL` / `ROR` | Giro sin carry | El bit que sale vuelve a entrar | MSB / LSB (copia) | No |

* `SHL` y `SAL` son idénticos.
* `SAR` conserva el signo: si el MSB es 1 (número negativo) se sigue inyectando 1.

**Ejemplos de la clase (referencia):**

| Operación | Binario | Resultado | `CF` |
|---|---|---|---|
| `SHR AX,1`, `AX=1234h` | `0001 0010 0011 0100` → `0000 1001 0001 1010` | `091Ah` | 0 |
| `SHL BX,2`, `BX=59ACh` | `…` → `B358h` (CF=0) → `66B0h` | `66B0h` | **1** |
| `SAR AX,2`, `AX=92CAh` | `1001 0010 1100 1010` → `C965h` (CF=0) → `1110 0100 1011 0010` | `E4B2h` | **1** |
| `RCR AX,2`, `AX=1234h`, CF previo=1 | `891Ah` (CF=0) → `448Dh` | `448Dh` | 0 |

## 4.2 Otras instrucciones del programa

| Instrucción | Función | Efecto en `CF` (8086) |
|---|---|---|
| `NOT d` | Invierte los bits | No cambia |
| `LOOP e` | **`CX ← CX − 1`**; salta a `e` si `CX ≠ 0`, si no continúa | No cambia |
| `INC d` | Suma 1 | **No cambia** |
| `ADD d,o` | `d ← d + o` | Sí cambia (acarreo) |
| `XCHG a,b` | Intercambia los contenidos | No cambia |
| `AND/OR/XOR` | Lógica bit a bit | Ponen `CF = 0` |
| `NEG d` | Complemento a dos (`10000h − d`) | Cambia |
| `JC e` / `JNC e` | Salta si `CF = 1` / si `CF = 0` | – |
| `JS e` / `JNS e` | Salta si `SF = 1` / si `SF = 0` | – |

* **`LOOP` es como DO-WHILE:** el cuerpo se ejecuta **al menos una vez**; con `MOV CX,2` el bloque `E1 … LOOP E1` se ejecuta **2 veces**.
* En cada vuelta `AX` y `BX` conservan el valor de la vuelta anterior (no vuelven al original).
* El `CF` que llega a `JNC/JC` es el que dejó la **última instrucción que lo modifica**, aquí `RCL BX,1` de la última vuelta (`NOT` y `LOOP` no lo cambian).
* **Saltos del examen:**

```asm
LOOP E1
JNC  E2      ; CF=0 → salta a E2 (ejecuta INC BX, INC AX y luego INC BX en E5)
JC   E5      ; CF=1 → (JNC no saltó) salta a E5 (ejecuta solo INC BX)
E: MOV CX,2  ; nunca se alcanza: siempre salta alguno de los dos
E2: INC BX
E3: INC AX
E5: INC BX
```

## 4.3 Método de resolución

1. Anotar `CX`, `AX`, `BX` iniciales y escribirlos en **binario** (4 bits por dígito).
2. Ejecutar el cuerpo del bucle **instrucción por instrucción**, escribiendo el binario, el hex y el `CF` tras cada una.
3. Al `LOOP`: restar 1 a `CX`; si es distinto de 0 repetir con los valores actuales.
4. Con el `CF` final decidir el salto (`JNC/JC`) y ejecutar solo las instrucciones del camino elegido.
5. Seguir con `ADD`, `SAR`, `XCHG`; **responder el primer cuadro** (valores intermedios, dan nota parcial).
6. Continuar con la última parte (`AND/OR/XOR` y `NEG`) usando esos mismos valores y responder el segundo cuadro.

## 4.4 Grupos B y C: el bucle (idéntico hasta `XCHG`)

Valores iniciales: `CX = 2`, `AX = 1930h = 0001 1001 0011 0000`, `BX = 0DBCEh = 1101 1011 1100 1110`.

**Vuelta 1 (`CX = 2`)**

| Instrucción | Operación en binario | Resultado | `CF` |
|---|---|---|---|
| `SHL AX,1` | `0001 1001 0011 0000` → `0011 0010 0110 0000` (sale el 0 del MSB, entra 0) | `AX = 3260h` | 0 |
| `SAR BX,1` | `1101 1011 1100 1110` → `1110 1101 1110 0111` (entra el signo `1`; sale el 0 del LSB) | `BX = EDE7h` | 0 |
| `RCR AX,1` | entra `CF=0` por el MSB: `0011 0010 0110 0000` → `0001 1001 0011 0000` (sale 0) | `AX = 1930h` | 0 |
| `RCL BX,1` | entra `CF=0` por el LSB: `1110 1101 1110 0111` → `1101 1011 1100 1110` (sale el 1 del MSB) | `BX = DBCEh` | **1** |
| `NOT AX` | `0001 1001 0011 0000` → `1110 0110 1100 1111` | `AX = E6CFh` | 1 |
| `LOOP E1` | `CX = 2 − 1 = 1` ≠ 0 → **salta a `E1`** | | |

**Vuelta 2 (`CX = 1`)**

| Instrucción | Operación en binario | Resultado | `CF` |
|---|---|---|---|
| `SHL AX,1` | `1110 0110 1100 1111` → `1100 1101 1001 1110` (sale el 1 del MSB) | `AX = CD9Eh` | **1** |
| `SAR BX,1` | `1101 1011 1100 1110` → `1110 1101 1110 0111` | `BX = EDE7h` | 0 |
| `RCR AX,1` | entra `CF=0`: `1100 1101 1001 1110` → `0110 0110 1100 1111` (sale 0) | `AX = 66CFh` | 0 |
| `RCL BX,1` | entra `CF=0`: `1110 1101 1110 0111` → `1101 1011 1100 1110` (sale 1) | `BX = DBCEh` | **1** |
| `NOT AX` | `0110 0110 1100 1111` → `1001 1001 0011 0000` | `AX = 9930h` | 1 |
| `LOOP E1` | `CX = 1 − 1 = 0` → **sale del bucle** | | |

**Después del bucle (`CF = 1`)**

| Instrucción | Procedimiento | Resultado |
|---|---|---|
| `JNC E2` | `CF = 1` → **no salta** | |
| `JC E5` | `CF = 1` → **salta a `E5`** (se omiten `INC BX` de `E2` y `INC AX`) | |
| `E5: INC BX` | `DBCEh + 1` | `BX = DBCFh` |
| `ADD AX,1` | `9930h + 1` | `AX = 9931h` |
| `SAR AX,01` | `1001 1001 0011 0001` → `1100 1100 1001 1000` (entra el signo `1`; sale el 1 del LSB) | `AX = CC98h`, `CF = 1` |
| `XCHG AH,BL` | `AH = CCh`, `BL = CFh` se intercambian | `AH = CFh`, `BL = CCh` → `AX = CF98h`, `BX = DBCCh` |

### Grupo C: continuación

**Primer cuadro:**

| AH | BH |
|---|---|
| **`CFh`** | **`DBh`** |

| Instrucción | Procedimiento | Resultado |
|---|---|---|
| `XOR BL,0AAH` | `CCh = 1100 1100` XOR `AAh = 1010 1010` → `0110 0110` | `BL = 66h` → `BX = DB66h` |
| `NEG AX` | `AX = CF98h`: invertir `0011 0000 0110 0111` + 1 = `0011 0000 0110 1000` (`10000h − CF98h`) | `AX = 3068h` |

| AX | BX |
|---|---|
| **`3068h`** | **`DB66h`** |

### Grupo B: continuación

**Primer cuadro (pide `AL` y `BL`):**

| AL | BL |
|---|---|
| **`98h`** | **`CCh`** |

| Instrucción | Procedimiento | Resultado |
|---|---|---|
| `AND AH,10111111B` | `AH = CFh = 1100 1111` AND `1011 1111` → `1000 1111` (solo se apaga el bit 6) | `AH = 8Fh` → `AX = 8F98h` |
| `NEG BX` | `BX = DBCCh = 1101 1011 1100 1100`: invertir `0010 0100 0011 0011` + 1 = `0010 0100 0011 0100` (`10000h − DBCCh`) | `BX = 2434h` |

| AX | BX |
|---|---|
| **`8F98h`** | **`2434h`** |

## 4.5 Grupo A: resolución completa

Valores iniciales: `CX = 2`, `AX = 1930h = 0001 1001 0011 0000`, `BX = 0FBC1h = 1111 1011 1100 0001`.

**Vuelta 1 (`CX = 2`)**

| Instrucción | Operación en binario | Resultado | `CF` |
|---|---|---|---|
| `SHL AX,1` | `0001 1001 0011 0000` → `0011 0010 0110 0000` | `AX = 3260h` | 0 |
| `SAR BX,1` | `1111 1011 1100 0001` → `1111 1101 1110 0000` (entra el signo `1`; **sale el 1 del LSB**) | `BX = FDE0h` | **1** |
| `RCR AX,1` | entra **`CF=1`** por el MSB: `0011 0010 0110 0000` → `1001 1001 0011 0000` (sale 0) | `AX = 9930h` | 0 |
| `RCL BX,1` | entra `CF=0`: `1111 1101 1110 0000` → `1111 1011 1100 0000` (sale 1) | `BX = FBC0h` | **1** |
| `NOT AX` | `1001 1001 0011 0000` → `0110 0110 1100 1111` | `AX = 66CFh` | 1 |
| `LOOP E1` | `CX = 1` ≠ 0 → salta | | |

**Vuelta 2 (`CX = 1`)**

| Instrucción | Operación en binario | Resultado | `CF` |
|---|---|---|---|
| `SHL AX,1` | `0110 0110 1100 1111` → `1100 1101 1001 1110` (sale el 0 del MSB) | `AX = CD9Eh` | 0 |
| `SAR BX,1` | `1111 1011 1100 0000` → `1111 1101 1110 0000` (sale 0) | `BX = FDE0h` | 0 |
| `RCR AX,1` | entra `CF=0`: `1100 1101 1001 1110` → `0110 0110 1100 1111` | `AX = 66CFh` | 0 |
| `RCL BX,1` | entra `CF=0`: `1111 1101 1110 0000` → `1111 1011 1100 0000` (sale 1) | `BX = FBC0h` | **1** |
| `NOT AX` | `0110 0110 1100 1111` → `1001 1001 0011 0000` | `AX = 9930h` | 1 |
| `LOOP E1` | `CX = 0` → **sale** | | |

**Después del bucle (`CF = 1` → `JC E5`)**

| Instrucción | Procedimiento | Resultado |
|---|---|---|
| `JNC E2` / `JC E5` | `CF=1`: no salta / salta a `E5` | |
| `E5: INC BX` | `FBC0h + 1` | `BX = FBC1h` |
| `ADD AX,1` | `9930h + 1` | `AX = 9931h` |
| `SAR AX,01` | `1001 1001 0011 0001` → `1100 1100 1001 1000` | `AX = CC98h` |
| `XCHG AH,BL` | `AH = CCh` ↔ `BL = C1h` | `AH = C1h`, `BL = CCh` → `AX = C198h`, `BX = FBCCh` |

**Primer cuadro:**

| AH | BH |
|---|---|
| **`C1h`** | **`FBh`** |

| Instrucción | Procedimiento | Resultado |
|---|---|---|
| `OR BH,10111100B` | `BH = FBh = 1111 1011` OR `1011 1100` → `1111 1111` | `BH = FFh` → `BX = FFCCh` |
| `NEG AX` | `AX = C198h = 1100 0001 1001 1000`: invertir `0011 1110 0110 0111` + 1 = `0011 1110 0110 1000` (`10000h − C198h`) | `AX = 3E68h` |

| AX | BX |
|---|---|
| **`3E68h`** | **`FFCCh`** |

## 4.6 Resumen del Ejercicio 3

| Grupo | Primer cuadro | `AX` final | `BX` final |
|---|---|---|---|
| **A** | `AH = C1h`, `BH = FBh` | **`3E68h`** | **`FFCCh`** |
| **B** | `AL = 98h`, `BL = CCh` | **`8F98h`** | **`2434h`** |
| **C** | `AH = CFh`, `BH = DBh` | **`3068h`** | **`DB66h`** |

## 4.7 Si el `CF` final fuera 0 (otras variantes)

* `JNC E2` **sí salta**: se ejecutan `E2: INC BX`, `E3: INC AX` y `E5: INC BX` (dos `INC BX` y un `INC AX`).
* Con `CF = 1` solo se ejecuta `E5: INC BX`.
* Si la variante cambia `MOV CX,2` por otro valor, el bucle da ese número de vueltas; si cambia el conteo de un desplazamiento (`SHL AX,2`) se repite el giro de 1 bit esas veces.

---

# PARTE 5. RESPUESTAS FINALES DEL EXAMEN

| Grupo | Ej. 1 (`ZZZZZ` / `yyyyy`) | Ej. 2 (`BX`, `AX`, celda, `SP`) | Ej. 3 (cuadro 1 → `AX`, `BX`) |
|---|---|---|---|
| **A** | `7FFFFh` / `42000h` | `1211h`, `1100h`, `[1FFD]=25h`, `2002h` | `AH=C1h, BH=FBh` → `3E68h`, `FFCCh` |
| **B** | `3FFFFh` / `04000h` | `11FFh`, `1215h`, `[1FFE]=15h`, `2003h` | `AL=98h, BL=CCh` → `8F98h`, `2434h` |
| **C** | `1FFFFh` / `06000h` | `1511h`, `1115h`, `[1FFD]=15h`, `2002h` | `AH=CFh, BH=DBh` → `3068h`, `DB66h` |

---

# PARTE 6. EXTRACTOS BREVES DE OTROS TEMAS

* **Von Neumann:** una sola memoria para instrucciones y datos; CPU = UC + ALU + registros; buses de direcciones, datos y control.
* **Registros 8086 (16 bits):** `AX, BX, CX, DX` (mitades `H`/`L`), `SP, BP, SI, DI`; segmentos `CS, DS, SS, ES`; `IP` apunta a la siguiente instrucción (`CS:IP`).
* **EU y BIU:** la unidad de ejecución ejecuta; la interfaz de bus busca y precarga (cola de 6 bytes en el 8086, 4 en el 8088).
* **Mapa del 8086 (1 MB, 20 bits):** RAM `00000h–9FFFFh` (640 KB), Video `A0000h–BFFFFh` (128 KB), ROM `C0000h–FFFFFh` (256 KB). Memoria máxima `= 2^(bits de dirección)`.
* **Banderas:** `CF` acarreo, `ZF` cero, `SF` signo (1 = negativo, resultado en complemento a dos).
* **Programas:** `.COM` (un segmento de 64 KB, `ORG 100h`) y `.EXE` (varios segmentos, `PROC FAR`, `ASSUME`); terminan con `MOV AX,4C00h` + `INT 21h`.
* **Conversión decimal → binario/hex:** divisiones sucesivas, residuos de abajo hacia arriba (`200 = C8h`); hex → decimal por potencias de 16 (`FAh = 15·16 + 10 = 250`).
* **Complemento a 1 / a 2:** invertir bits / invertir + 1; resta `M − N + rⁿ` (si hay acarreo se descarta y el resultado es positivo).
* **BCD, Gray, ASCII:** BCD = 4 bits por dígito decimal (`27 = 0010 0111`); Gray: `G(i) = B(i) XOR B(i−1)`; ASCII de 7 bits = 128 caracteres (`A = 1000001`); bit de paridad par/impar.
* **Álgebra booleana:** `AND`, `OR`, `NOT`; `NAND` y `NOR` son universales; medio sumador `S = x ⊕ y`, `C = x·y`; sumador completo `S = x ⊕ y ⊕ z`, `C = xy + z(x ⊕ y)`.
* **Mapas de Karnaugh:** orden Gray (`00,01,11,10`), grupos de 2/4/8 celdas eliminan 1/2/3 variables, los bordes son adyacentes, `X` (don't care) se usa solo si agranda un grupo.
* **Evolución Intel:** 4004 (1971, 4 bits) → 8008 (1972, 8 bits, 16 KB) → 8086 (1978, 16 bits, 1 MB) → 80286 (1982, 16 MB) → 80386 (1985, 32 bits, 4 GB).

---

# PARTE 7. LISTA DE CONTROL PARA EL EXAMEN

1. **Ej. 1:** `Xf = 2ⁿ − 1` para `ZZZZZ`; para `yyyyy`: `(TAM − 1) + 02000h`, luego **+ 1**.
2. **Ej. 2:** convertir el mapa a **bytes**; `PUSH`: `SP−2`, alto en `SP+1`, bajo en `SP`; `POP`: leer `[SP]` bajo y `[SP+1]` alto, `SP+2`.
3. **Ej. 2:** destino de 8 bits lee **1 byte**; de 16 bits lee **2 bytes**; leer siempre del **estado actual** de la memoria.
4. **Ej. 3:** trabajar en **binario**, un bit a la vez, anotando `CF` después de cada instrucción.
5. **Ej. 3:** `RCR/RCL` usan el `CF` anterior; `SAR` repite el signo; `LOOP` resta 1 a `CX` y repite si `CX ≠ 0`.
6. **Ej. 3:** elegir el camino de `JNC/JC` con el `CF` de la **última** instrucción que lo modifica (`RCL BX,1`).
7. `NEG x = 10000h − x` (invertir + 1); verificar `x + NEG(x) = 10000h`.
8. Responder cada cuadro **intermedio** aunque el resultado final falle: los parciales dan nota.

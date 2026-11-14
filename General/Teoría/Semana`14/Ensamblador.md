# Assembly: Pila (Stack), PUSH/POP, desplazamientos, rotaciones, LOOP y saltos

---

## 1. Segmento de pila (Stack)

| Concepto | Explicación |
|---|---|
| **Pila (stack)** | Segmento de memoria de hasta **64 KB** donde la CPU guarda **temporalmente** información de sus registros |
| **Para qué sirve** | Ante una **interrupción** (hardware o software, ej. Enter, Escape) la CPU deja lo que hace, **guarda** su información actual en la pila (`PUSH`), atiende la interrupción y luego la **recupera** (`POP`) |
| **Celda** | Cada celda es **1 byte (8 bits)** y tiene su propia dirección |
| **`SS`** | Registro de segmento de pila: indica **dónde empieza** la pila (16 bits, se le agrega un `0` a la derecha para formar 5 dígitos hex) |
| **`SP`** | **Stack Pointer**: registro de 16 bits con el **desplazamiento** (offset) dentro de la pila |
| **`SS:SP`** | Dirección lógica del tope de la pila. Ej.: `0100:1234H` → inicio `01000h` + desplazamiento `1234h` = `02234h` |
| **Analogía de la clase** | Una vez conocido el punto de referencia (inicio del segmento), solo se dice "a 200 m del punto A": el ensamblador trabaja **solo con el desplazamiento** |

### Organización de la CPU (16 bits, 14 registros)

| Grupo | Registros | Detalle |
|---|---|---|
| **EU** (ejecución) | `AX, BX, CX, DX` | Cada uno = 2 bytes: parte **alta** (`AH, BH, CH, DH`) y parte **baja** (`AL, BL, CL, DL`) |
| **BIU** (bus) | `CS, DS, SS, ES` (+ `SP`) | Segmentos y apuntadores |

* La división en `H` y `L` es **lógica, no física**: se puede trabajar con 16 bits (`AX`) o con 8 bits (`AH` o `AL`).
* Un registro de 16 bits = **4 dígitos hexadecimales**; cada dígito hex = 4 bits binarios (ej. `1234h` = `0001 0010 0011 0100`).

### Reglas de escritura en el emulador (emu8086)

| Regla | Ejemplo |
|---|---|
| Sin sufijo el número se toma como **decimal** (muy grande si era hex) → **siempre** poner `H` para hexadecimal | `MOV SP,2003H` |
| Número hexadecimal que **empieza con letra** lleva un `0` delante (si no, el emulador da error) | `0FF89H`, `0DBCEH`, `0AAH` |
| Binario con sufijo `B` (si no se pone, se toma como decimal enorme y hay desbordamiento) | `AND AH,11111111B` |
| `MOV SP,1234H` es **direccionamiento inmediato**: carga el valor directamente en el registro | `MOV AX,1239H` |
| Los **corchetes `[ ]`** indican **un lugar en la memoria** | `SS:[2000H]` |
| Las **etiquetas** llevan `:` pegado, no se ejecutan y solo sirven para ordenar/saltar | `E1:` |

---

## 2. Leer la memoria de pila con `MOV`

Ejemplo: `MOV BX,SS:[2000H]`

| Regla | Explicación |
|---|---|
| **1. Dirección → CPU** | Copia el **contenido** de un lugar de la memoria a un registro. La memoria **no cambia** |
| **2. Hay que escribir `SS:`** | Si no se menciona `SS`, el ensamblador usa por defecto el **segmento de datos (`DS`)** |
| **3. El tamaño lo decide el destino** | Destino de **8 bits** (`BH, BL, AH, AL…`) → lee **1 byte** de la dirección indicada. Destino de **16 bits** (`BX, AX…`) → lee **2 bytes** |
| **4. Orden de los 2 bytes** | La dirección escrita (ej. `2000`) es la **más baja** y va a la **parte baja** (`BL`); la **siguiente** (`2001`) va a la **parte alta** (`BH`) |
| **5. Direcciones** | Crecen hacia arriba en **hexadecimal**: `1FFD, 1FFE, 1FFF, 2000, 2001, 2002…` |

### Ejemplo visual de la clase

Memoria de pila: `2001 = 22`, `2000 = 9A`, `1FFF = 38`, `1FFE = 23`.

| Instrucción | Qué lee | Resultado |
|---|---|---|
| `MOV BX,SS:[2000H]` | `[2000]=9A` → `BL`, `[2001]=22` → `BH` | `BX = 229Ah` |
| `MOV BH,SS:[2000H]` | Solo `[2000]=9A` (destino de 1 byte) | `BH = 9Ah` (`BL` no cambia) |

---

## 3. `PUSH` (guardar en la pila)

**`PUSH AX`**: **copia** temporalmente el contenido de `AX` a la pila. `AX` **no cambia**.

### Procedimiento (siempre en este orden)

1. **`SP ← SP − 2`** (en hexadecimal): el puntero **baja** 2 posiciones.
2. Se guarda `AH` en la dirección **alta** (`SP+1`) y `AL` en la dirección **baja** (`SP`).
3. **No se toca** la celda donde apuntaba el `SP` antes del `PUSH`.

### Ejemplo de la clase

`MOV SP,2000H`, `AX = 5348H` (`AH=53`, `AL=48`). Memoria antes: `2002=93, 2001=81, 2000=43, 1FFF=A9, 1FFE=2B, 1FFD=1C`.

| Paso | Resultado |
|---|---|
| `SP = 2000h − 2` | `SP = 1FFEh` |
| `AH = 53` → dirección alta | `[1FFF] = 53` |
| `AL = 48` → dirección baja | `[1FFE] = 48` |
| Celda `2000` | **Sin cambio** (`43`) |
| `AX` | **Sin cambio** (`5348h`) |

---

## 4. `POP` (recuperar de la pila)

**`POP BX`**: **recupera** de la pila 2 bytes y los copia en `BX`.

### Procedimiento (orden distinto a PUSH)

1. Se **leen** los 2 bytes de **donde apunta el `SP`**: `[SP]` → parte **baja**, `[SP+1]` → parte **alta** del destino.
2. **`SP ← SP + 2`** (el puntero sube).
3. La memoria **no se borra ni cambia**: los datos leídos quedan ahí.

### Ejemplo de la clase

`MOV SP,2000H`, `BX = 1234h`. Memoria: `2003=82, 2002=99, 2001=2C, 2000=4A, 1FFF=25`.

| Paso | Resultado |
|---|---|
| Lee `[2000]=4A` | → `BL = 4A` |
| Lee `[2001]=2C` | → `BH = 2C` |
| `SP = 2000h + 2` | `SP = 2002h` |
| **Resultado** | `BX = 2C4Ah`; la memoria queda sin cambio (`2002 = 99` no se toca) |

### Diferencias PUSH vs POP

| | `PUSH` | `POP` |
|---|---|---|
| Mueve `SP` | **Baja** 2 (antes de escribir) | **Sube** 2 (después de leer) |
| Celda a la que apuntaba `SP` | **No importa / no se toca** | **Sí importa**: se lee |
| Destino | Memoria de pila | Registro |
| Orden de bytes | Alto → `SP+1`, bajo → `SP` | `[SP+1]` → alto, `[SP]` → bajo |

**Fórmula del SP final:** `SP final = SP inicial − 2·(#PUSH) + 2·(#POP)`.

---

## 5. Cómo leer la ventana `stack` del emulador

* Cada fila es `0100:XXXX  VVVV`: `0100` = `SS`, `XXXX` = dirección, `VVVV` = **palabra de 2 bytes**.
* En la palabra, los **2 dígitos de la derecha** son el byte de **esa dirección** (bajo) y los **2 de la izquierda** el de la **dirección + 1** (alto).
* Ejemplo: fila `0100:2001  1239` → `[2001]=39`, `[2002]=12`.
* Por defecto el `SP` del emulador es `FFFEh`; `MOV SP,2003H` lo reubica.
* Uso: `emulate` → `single step` (paso a paso) → `stack` y `flags` para ver cambios.

---

## 6. Ejemplo resuelto de pila (tipo Pregunta 2)

*(Valores iniciales de registros y pila = 0)*

```asm
MOV SP,2003H
MOV AX,1239H
PUSH AX
MOV AX,0FF89H
PUSH AX
MOV BX,SS:[2000H]
PUSH BX
MOV AX,1024H
PUSH AX
MOV CX,91F4H
POP CX
MOV BX,SS:[2000H]
POP BX
                      ; ¿CL?
MOV CX,SS:[1FFFH]
PUSH CX
MOV BH,SS:[1FFFH]
MOV DX,SS:[1FFEH]
MOV AX,4567H
PUSH AX
MOV AX,1C29H
PUSH AX
MOV AX,0BEA1H
POP AX
MOV BL,SS:[1FFFH]
MOV BX,SS:[1FFCH]
                      ; ¿AX, BX, SP?
```

### Traza paso a paso

| # | Instrucción | Qué ocurre | SP | Registros / memoria |
|---|---|---|---|---|
| 1 | `MOV AX,1239H` | Inmediato | 2003 | `AX=1239` |
| 2 | `PUSH AX` | SP−2; `[2002]=12`, `[2001]=39` | **2001** | |
| 3 | `MOV AX,0FF89H` | Inmediato | 2001 | `AX=FF89` |
| 4 | `PUSH AX` | `[2000]=FF`, `[1FFF]=89` | **1FFF** | |
| 5 | `MOV BX,SS:[2000H]` | `[2000]=FF`→BL, `[2001]=39`→BH | 1FFF | `BX=39FF` |
| 6 | `PUSH BX` | `[1FFE]=39`, `[1FFD]=FF` | **1FFD** | |
| 7 | `MOV AX,1024H` | Inmediato | 1FFD | `AX=1024` |
| 8 | `PUSH AX` | `[1FFC]=10`, `[1FFB]=24` | **1FFB** | |
| 9 | `MOV CX,91F4H` | Inmediato | 1FFB | `CX=91F4` |
| 10 | `POP CX` | Lee `[1FFB]=24`(bajo), `[1FFC]=10`(alto); SP+2 | **1FFD** | `CX=1024` → **`CL = 24h`** |
| 11 | `MOV BX,SS:[2000H]` | `[2000]=FF`, `[2001]=39` | 1FFD | `BX=39FF` |
| 12 | `POP BX` | Lee `[1FFD]=FF`, `[1FFE]=39`; SP+2 | **1FFF** | `BX=39FF` |
| 13 | `MOV CX,SS:[1FFFH]` | `[1FFF]=89`(bajo), `[2000]=FF`(alto) | 1FFF | `CX=FF89` |
| 14 | `PUSH CX` | `[1FFE]=FF`, `[1FFD]=89` | **1FFD** | |
| 15 | `MOV BH,SS:[1FFFH]` | 1 byte: `[1FFF]=89` → BH | 1FFD | `BX=89FF` |
| 16 | `MOV DX,SS:[1FFEH]` | `[1FFE]=FF`, `[1FFF]=89` | 1FFD | `DX=89FF` |
| 17 | `MOV AX,4567H` | Inmediato | 1FFD | `AX=4567` |
| 18 | `PUSH AX` | `[1FFC]=45`, `[1FFB]=67` | **1FFB** | |
| 19 | `MOV AX,1C29H` | Inmediato | 1FFB | `AX=1C29` |
| 20 | `PUSH AX` | `[1FFA]=1C`, `[1FF9]=29` | **1FF9** | |
| 21 | `MOV AX,0BEA1H` | Inmediato | 1FF9 | `AX=BEA1` |
| 22 | `POP AX` | Lee `[1FF9]=29`, `[1FFA]=1C`; SP+2 | **1FFB** | `AX=1C29` |
| 23 | `MOV BL,SS:[1FFFH]` | 1 byte: `89` → BL | 1FFB | `BX=8989` |
| 24 | `MOV BX,SS:[1FFCH]` | `[1FFC]=45`(bajo), `[1FFD]=89`(alto) | 1FFB | `BX=8945` |

### Respuestas

| Pregunta | Valor |
|---|---|
| **CL** (tras el `POP BX`) | **`24h`** |
| **AX** | **`1C29h`** |
| **BX** | **`8945h`** |
| **SP** | **`1FFBh`** |

### Mapa final de la pila (coincide con la ventana `stack` del emulador)

| Dirección | Byte | Palabra mostrada |
|---|---|---|
| `2002` / `2001` | `12` / `39` | `2001 → 1239` |
| `2000` / `1FFF` | `FF` / `89` | `1FFF → FF89` |
| `1FFE` / `1FFD` | `FF` / `89` | `1FFD → FF89` |
| `1FFC` / `1FFB` | `45` / `67` | `1FFB → 4567` ← `SP` |
| `1FFA` / `1FF9` | `1C` / `29` | `1FF9 → 1C29` |

### Segundo ejemplo: variante con `SP = 2001H` (direcciones nunca escritas valen `00`)

Instrucciones: `MOV SP,2001H`, `AX=9209H` push, `AX=0F189H` push, `MOV BX,SS:[2001H]`, push BX, `AX=1524H` push, `CX=91F4H`, pop CX, `MOV BX,SS:[2002H]`, pop BX. Luego: `MOV CX,SS:[1FFEH]`, push CX, `MOV BH,SS:[1FFDH]`, `MOV DX,SS:[1FFCH]`, `AX=4567H` push, `AX=1C29H` push, `AX=0BEA4H`, pop AX, `MOV AL,SS:[1FFDH]`, `MOV BX,SS:[1FFCH]`.

| Punto | Procedimiento | Resultado |
|---|---|---|
| `PUSH AX (9209)` | SP=1FFF; `[2000]=92`, `[1FFF]=09` | |
| `PUSH AX (F189)` | SP=1FFD; `[1FFE]=F1`, `[1FFD]=89` | |
| `MOV BX,SS:[2001H]` | `[2001]` y `[2002]` **nunca se escribieron** → `00` | `BX=0000` |
| `PUSH BX` | SP=1FFB; `[1FFC]=00`, `[1FFB]=00` | |
| `PUSH AX (1524)` | SP=1FF9; `[1FFA]=15`, `[1FF9]=24` | |
| `POP CX` | `[1FF9]=24`, `[1FFA]=15`; SP=1FFB | `CX=1524` → **`CH = 15h`** |
| `POP BX` | `[1FFB]=00`, `[1FFC]=00`; SP=1FFD | `BX=0000` |
| `MOV CX,SS:[1FFEH]` | `[1FFE]=F1`, `[1FFF]=09` | `CX=09F1` |
| `PUSH CX` | SP=1FFB; `[1FFC]=09`, `[1FFB]=F1` | |
| `MOV BH,SS:[1FFDH]` / `MOV DX,SS:[1FFCH]` | `[1FFD]=89` / `[1FFC]=09,[1FFD]=89` | `BX=8900`, `DX=8909` |
| `PUSH AX (4567)` | SP=1FF9; `[1FFA]=45`, `[1FF9]=67` | |
| `PUSH AX (1C29)` | SP=1FF7; `[1FF8]=1C`, `[1FF7]=29` | |
| `POP AX` | `[1FF7]=29`, `[1FF8]=1C`; SP=1FF9 | `AX=1C29` |
| `MOV AL,SS:[1FFDH]` | 1 byte `89` → AL | `AX=1C89` |
| `MOV BX,SS:[1FFCH]` | `[1FFC]=09`, `[1FFD]=89` | `BX=8909` |
| **Respuesta final** | | **`CH=15h`, `AX=1C89h`, `BX=8909h`, `SP=1FF9h`** |

---

## 7. `LOOP`, `CX` y etiquetas

| Concepto | Explicación |
|---|---|
| **`CX`** | Tiene **dos funciones**: registro general (como `AX`, `BX`) y **contador de bucle**. Dentro de un loop hay que tener cuidado al usarlo |
| **Estructura** | `E1: ... instrucciones ... LOOP E1` |
| **Funcionamiento (como DO-WHILE)** | El cuerpo se ejecuta **al menos una vez**. Al llegar a `LOOP E1`: **1)** `CX ← CX − 1`; **2)** si `CX ≠ 0` salta a `E1`; si `CX = 0` **sale** |
| **Cantidad de vueltas** | `MOV CX,3` → el cuerpo se ejecuta **3 veces** |
| **Valores acumulados** | En cada vuelta `AX`, `BX`… **conservan el valor de la vuelta anterior**, no el original |
| **Etiquetas** | `nombre:` con los dos puntos pegados; no ejecutan nada, solo marcan un punto al que se salta |

---

## 8. Banderas (flags) y saltos condicionales

### Banderas importantes (registro de banderas de 16 bits, modificado por la ALU)

| Bandera | Se pone en 1 cuando… |
|---|---|
| **CF** (Carry) | Hay **acarreo** en una operación (en desplazamientos/rotaciones = **último bit que salió**) |
| **ZF** (Zero) | El **resultado es cero** (ej. `2 − 2`) |
| **SF** (Sign) | El resultado es **negativo** (la CPU usa **complemento a dos**: `SF=1` y guarda el resultado en complemento a dos) |
| OF, PF, etc. | No son importantes en esta clase |

* El **acarreo del sistema** es **un solo lugar** (no es parte del registro): cada instrucción que lo manipula **sobrescribe** el valor anterior.

### Saltos condicionales

| Instrucción | Salta a la etiqueta si… |
|---|---|
| `JC etiqueta` | `CF = 1` |
| `JNC etiqueta` | `CF = 0` |
| `JS etiqueta` | `SF = 1` |
| `JNS etiqueta` | `SF = 0` |

* Si la condición **no se cumple**, la instrucción se **ignora** y se ejecuta la siguiente línea.
* La etiqueta puede estar arriba o abajo en el programa.
* Patrón del examen: `JC E2` seguido de `JNC E` → con `CF=1` salta a `E2`; con `CF=0` no salta, pasa a `JNC E`, que sí salta a `E` (`MOV CX,2`) y de ahí cae en `E2`.

---

## 9. Instrucciones usadas en el programa

| Instrucción | Función | Ejemplo y efecto |
|---|---|---|
| `MOV d,o` | Copia origen en destino | `MOV CX,3` |
| `INC d` | Suma 1 al destino | `INC BX` |
| `ADD d,o` | `d ← d + o` | `ADD AX,1` |
| `XCHG a,b` | **Intercambia** el contenido de ambos | `XCHG AH,BL` |
| `AND d,o` | Y lógico bit a bit (resultado en `d`) | `AND AH,11111111B` |
| `OR d,o` | O lógico bit a bit (resultado en `d`) | `OR BH,AH` |
| `XOR d,o` | O exclusivo bit a bit (resultado en `d`) | `XOR BH,0AAH` |
| `NOT d` | **Invierte** todos los bits (1→0, 0→1) | `NOT AX` |
| `NEG d` | **Complemento a dos** (`NOT` + 1) | `NEG AX` |
| `LOOP e` | `CX−1`; salta si `CX≠0` | `LOOP E1` |

---

## 10. Desplazamientos y rotaciones

*(El dibujo de las flechas se entrega en el examen; no se memoriza. Lo que se evalúa es el procedimiento.)*

### Regla general

* El número de veces (`AX,2`) significa **repetir el movimiento de 1 bit, una vez tras otra, sobre el último valor**; nunca se mueven 2 bits de golpe.
* **`CF` queda con el último bit que salió** por el extremo.
* Todos manipulan el acarreo del sistema.
* Escribir en binario (4 bits por dígito hex) y mover bit a bit en la hoja.

| Instrucción | Dirección | Entra por el otro extremo | Sale a `CF` | ¿Usa `CF` anterior? |
|---|---|---|---|---|
| **`SHR`** | Derecha | Se **inyecta `0`** por el MSB | LSB | No |
| **`SHL` / `SAL`** | Izquierda | Se **inyecta `0`** por el LSB | MSB | No |
| **`SAR`** | Derecha (aritmético) | **Se repite el bit de signo** (MSB), no se inyecta 0 | LSB | No |
| **`ROL`** | Giro izquierda | El bit que sale (MSB) **vuelve a entrar** por el LSB | MSB (copia) | **No participa** (se sobrescribe) |
| **`ROR`** | Giro derecha | El LSB que sale **vuelve** por el MSB | LSB (copia) | **No participa** |
| **`RCL`** | Giro izquierda **con carry** | **`CF` anterior** entra por el LSB | MSB | **Sí** |
| **`RCR`** | Giro derecha **con carry** | **`CF` anterior** entra por el MSB | LSB | **Sí** |

* `SHL` y `SAL` son **idénticos**.
* "Aritmético" (`SAL`/`SAR`) significa que se considera el **bit de signo** (MSB = 1 → negativo).
* En `RCL`/`RCR` el `CF` anterior **sí influye**, por eso hay que llevar el valor del carry instrucción por instrucción.

### Ejemplos resueltos (los de la pizarra)

| Operación | Binario paso a paso | Resultado | `CF` |
|---|---|---|---|
| `SHR AX,1` con `AX=1234h` | `0001 0010 0011 0100` → `0000 1001 0001 1010` (entra 0 a la izquierda) | `AX = 091Ah` | `0` |
| `SHL BX,2` con `BX=59ACh` | `0101 1001 1010 1100` → (1) `1011 0011 0101 1000` = `B358h`, CF=0 → (2) `0110 0110 1011 0000` | `BX = 66B0h` | **`1`** |
| `SAR AX,2` con `AX=92CAh` | `1001 0010 1100 1010` → (1) `1100 1001 0110 0101` = `C965h`, CF=0 → (2) `1110 0100 1011 0010` (se repite el `1` del signo) | `AX = E4B2h` | **`1`** |
| `ROL AX,1` con `AX=1234h` (CF previo = 1) | `0001 0010 0011 0100` → `0010 0100 0110 1000` (el 0 que sale va a CF y entra por LSB; el CF previo se pierde) | `AX = 2468h` | `0` |
| `RCR AX,2` con `AX=1234h` y CF previo = 1 | (1) entra CF=1 por MSB: `1000 1001 0001 1010` = `891Ah`, CF=0 → (2) entra CF=0: `0100 0100 1000 1101` | `AX = 448Dh` | `0` |

---

## 11. Ejemplo completo resuelto (tipo Pregunta 3)

```asm
MOV CX,3
MOV AX,1230H
MOV BX,0DBCEH
E1: SHL AX,1
    SAL AX,1
    SAR BX,1
    RCR AX,2        ; CARRY
    RCL BX,1
    NOT AX
    LOOP E1
    JC  E2
    JNC E
E:  MOV CX,2
E2: INC BX
E3: INC AX
E4: INC BX
    SHL AX,01
    ADD AX,1
    SAR BX,01
    XCHG AH,BL
                    ; ¿AH, BH, CH?
    AND AH,11111111B
    OR  BH,AH
    XOR BH,0AAH
    NEG AX
                    ; ¿AX, BX?
```

### Bucle (3 vueltas)

| Vuelta | Instrucción | AX | BX | CF |
|---|---|---|---|---|
| **1** (`CX=3`) | Inicio | `1230` | `DBCE` | – |
| | `SHL AX,1` | `2460` | | 0 |
| | `SAL AX,1` | `48C0` | | 0 |
| | `SAR BX,1` | | `EDE7` | 0 |
| | `RCR AX,2` (entra CF=0 dos veces) | `1230` | | 0 |
| | `RCL BX,1` (entra CF=0) | | `DBCE` | **1** |
| | `NOT AX` | `EDCF` | | 1 |
| | `LOOP`: `CX=2` ≠ 0 → salta a `E1` | | | |
| **2** (`CX=2`) | `SHL AX,1` | `DB9E` | | 1 |
| | `SAL AX,1` | `B73C` | | 1 |
| | `SAR BX,1` | | `EDE7` | 0 |
| | `RCR AX,2` | `2DCF` | | 0 |
| | `RCL BX,1` | | `DBCE` | **1** |
| | `NOT AX` | `D230` | | 1 |
| | `LOOP`: `CX=1` ≠ 0 → salta | | | |
| **3** (`CX=1`) | `SHL AX,1` | `A460` | | 1 |
| | `SAL AX,1` | `48C0` | | 1 |
| | `SAR BX,1` | | `EDE7` | 0 |
| | `RCR AX,2` | `1230` | | 0 |
| | `RCL BX,1` | | `DBCE` | **1** |
| | `NOT AX` | `EDCF` | | 1 |
| | `LOOP`: `CX=0` → **sale** | | | |

### Después del bucle

| Instrucción | Procedimiento | Resultado |
|---|---|---|
| `JC E2` | `CF=1` → **salta a `E2`** (no se ejecuta `MOV CX,2`, por eso `CX` queda en `0`) | |
| `E2: INC BX` | `DBCE + 1` | `BX=DBCF` |
| `E3: INC AX` | `EDCF + 1` | `AX=EDD0` |
| `E4: INC BX` | `DBCF + 1` | `BX=DBD0` |
| `SHL AX,01` | `EDD0` → `DBA0` (CF=1) | `AX=DBA0` |
| `ADD AX,1` | | `AX=DBA1` |
| `SAR BX,01` | `1101 1011 1101 0000` → `1110 1101 1110 1000` | `BX=EDE8` |
| `XCHG AH,BL` | `AH=DB` ↔ `BL=E8` | `AX=E8A1`, `BX=EDDB` |

**Respuesta parte 1:** `AH = E8h`, `BH = EDh`, `CH = 00h`.

### Parte final

| Instrucción | Procedimiento | Resultado |
|---|---|---|
| `AND AH,11111111B` | `E8 AND FF` | `AH = E8` |
| `OR BH,AH` | `1110 1101 OR 1110 1000` | `BH = ED` |
| `XOR BH,0AAH` | `1110 1101 XOR 1010 1010 = 0100 0111` | `BH = 47` → `BX = 47DB` |
| `NEG AX` | `10000h − E8A1h` (complemento a dos) | `AX = 175F` |

**Respuesta final:** `AX = 175Fh`, `BX = 47DBh`.

---

## 12. Qué cambia en las variantes de examen

| Variante | Cómo afecta |
|---|---|
| `SP` inicial distinto (`2000h, 2001h, 2003h, 2004h, 2006h`) | Todas las direcciones de los `PUSH` y el `SP` final se desplazan; recalcular desde ese valor |
| `MOV reg,SS:[dirección]` con dirección **impar** o que cruza dos `PUSH` | Se lee desde esa dirección (bajo) y la siguiente (alto); mezcla bytes de `PUSH` distintos |
| Destino de 8 bits (`BH, BL, AL…`) | Lee **solo 1 byte** y la otra mitad del registro **no cambia** |
| Dirección nunca escrita | Vale **`00`** (la pila inicia en ceros) |
| `POP` después de `MOV reg,valor` | El `POP` **sobrescribe** el registro |
| Pregunta por `CL` o `CH` | Es la mitad baja o alta de `CX` tras el `POP` (`CH` = byte alto, `CL` = byte bajo) |
| `SHL AX,2`, `RCR AX,1`, `RCL BX,2` | Repetir el giro de 1 bit el número de veces indicado, llevando el `CF` |
| `MOV CX,2` inicial | El bucle da **2 vueltas** en vez de 3 |
| `JC E` / `JNC E2` y `E2: INC BX` + `LOOP E2` | Con `CF=1` entra a `E` (`CX=2`) y `LOOP E2` repite `INC BX` 2 veces; con `CF=0` va directo a `E2` con `CX=0` y `LOOP` baja a `FFFFh` (65 536 vueltas) |
| `XCHG AH,BL` / `AH,CH` / `AH,BH` | Cambia qué mitades se intercambian |
| `AND AH,máscara` | Convertir la máscara binaria y aplicar bit a bit (`AND`: solo queda 1 donde ambos son 1) |
| `NEG BX` en lugar de `NEG AX` | El complemento a dos se aplica al otro registro |
| `XOR BH,AAH` / `ABH` | Operar con el byte hexadecimal indicado |

---

## 13. Procedimiento recomendado para resolver

1. Preparar una hoja y llevar una **tabla de registros** (`AX, BX, CX, DX, SP`) y la **memoria de pila** por dirección (byte a byte).
2. Pila: anotar `SP`; en cada `PUSH` bajar 2 y escribir `AH` en `SP+1`, `AL` en `SP`; en cada `POP` leer `[SP]` (bajo) y `[SP+1]` (alto) y subir 2.
3. En cada `MOV reg,SS:[dir]`: mirar el **tamaño del destino** (1 o 2 bytes) y leer desde `dir`.
4. Desplazamientos y rotaciones: pasar a **binario**, mover **bit a bit**, repetir las veces indicadas y anotar el **`CF`** después de cada instrucción.
5. En `LOOP`: restar 1 a `CX` y repetir mientras sea distinto de cero, arrastrando los valores de `AX` y `BX` de la vuelta anterior.
6. En `JC/JNC/JS/JNS`: revisar la bandera y decidir si salta o continúa.
7. Responder los valores intermedios pedidos (`AH`, `BH`, `CH`…) y **seguir con esos mismos valores** hasta el final del programa; los intermedios correctos dan nota parcial.

# Arquitectura de computadoras II: evolución de los microprocesadores Intel y arquitectura del 8086/8088

## 1. Primera calculadora: el ábaco

* El **ábaco** es considerado la **calculadora primitiva**, hecha con **cuentas o esferas** ensartadas en varillas.
* Se ubica alrededor del año **500 a. C.**
* Cada fila representa un orden de magnitud (unidades, decenas, centenas, etc.) y se calcula **desplazando las cuentas** manualmente.
* **Problema:** **no es programable**. Depende totalmente de la persona que lo opera y no puede almacenar ni ejecutar instrucciones por sí mismo.
* Esta limitación marca el punto de partida de la computación: el objetivo posterior fue crear máquinas **programables** capaces de ejecutar secuencias de instrucciones, lo que dio origen al microprocesador.

---

## 2. Evolución de los microprocesadores Intel (línea de tiempo)

| Procesador | Año | Bits de datos | Bus de direcciones | Memoria máxima |
|---|:--:|:--:|:--:|:--:|
| **Intel 4004** (predecesor de 4 bits) | 1971 | 4 bits | 12 bits | 4 KB |
| **Intel 8008** | 1972 | 8 bits | 14 bits | 16 KB |
| **Intel 8086 / 8088** | 1978 / 1979 | 16 bits (el 8088 usa bus externo de 8 bits) | 20 bits | 1 MB |
| **Intel 80286** | 1982 | 16 bits | 24 bits | 16 MB |
| **Intel 80386** | 1985 | 32 bits | 32 bits (DX) / 24 bits (SX) | 4 GB (DX) / 16 MB (SX) |

* **Relación entre bits de dirección y memoria:** `memoria máxima = 2^(bits de dirección)` bytes.

```
8008  : 2^14 = 16 384 bytes      = 16 KB
8086  : 2^20 = 1 048 576 bytes   = 1 MB
80286 : 2^24 = 16 777 216 bytes  = 16 MB
80386 : 2^32 = 4 294 967 296 B   = 4 GB
```

---

## 3. Segundo microprocesador de Intel: el 8008

| Característica | Detalle |
|---|---|
| **Tipo de CPU** | 8 bits |
| **Origen** | Versión ampliada del 4004, pero con **8 bits** |
| **Direccionamiento** | Direcciones de **14 bits** → hasta **16 KB** |
| **Año** | **1972** |
| **Velocidad de CPU** | **0,5 MHz ~ 0,8 MHz** |
| **Encapsulado (socket)** | **18 pines DIP** |

* El 8008 procesaba **8 bits de datos al mismo tiempo** y podía **acceder a mucha más memoria** que sus predecesores.
* Por ello fue, en la práctica, entre **tres y cuatro veces más rápido** que los procesadores de 4 bits anteriores.
* Es un **chip de 18 pines**, lo que obligaba a multiplexar parte de sus señales (limitación física del encapsulado).

---

## 4. Procesadores de 16 bits: Intel 80286

### Características generales

| Característica | Detalle |
|---|---|
| **Nombre oficial** | **iAPX 286** (también i286 o 286) |
| **Tipo de CPU** | **16 bits**, de la familia **x86** |
| **Lanzamiento** | **1982** |
| **Transistores** | **134 000** |
| **Velocidad inicial** | **6 y 8 MHz** |
| **Velocidad máxima** | Hasta **25 MHz** |
| **Bus de direcciones** | **24 bits** |
| **Memoria direccionable** | **16 MB** de RAM (el 8086 solo puede **1 MB**) |

### Rendimiento

* Tras las versiones iniciales de 6 y 8 MHz, Intel lanzó un modelo a **12,5 MHz**.
* En promedio, el 80286 ejecutaba unas **0,21 instrucciones por ciclo de reloj**.

| Frecuencia del modelo | Rendimiento (según la diapositiva) |
|:--:|:--:|
| 6 MHz | 0,9 MIPS |
| 10 MHz | 1,5 MIPS |
| 12 MHz | 2,66 MIPS |

* **MIPS** = millones de instrucciones por segundo.

### Importancia histórica

* Fue el microprocesador elegido para equipar al **IBM Personal Computer/AT**.
* Por eso fue el **más empleado en los compatibles PC** (más propiamente *compatibles AT*) entre **mediados y finales de los años 80**.

---

## 5. Procesadores de 32 bits: Intel 80386

### Características generales

* **CPU de 32 bits.** Es el sucesor del i286 y se conoce como **Intel 80386DX**.
* Fabricado y diseñado por Intel, fue lanzado al mercado el **16 de octubre de 1985**.

### Variantes

| Variante | Descripción |
|---|---|
| **80386DX** | Bus de **32 bits** en dirección y datos; hasta **4 GB** de direccionamiento; **16 MHz a 40 MHz** |
| **80386SX** | Arquitectura **interna de 32 bits**, pero **bus externo de 16 bits** (situación parecida al 8086 y al 8088 en los primeros PC). Bus de direcciones de **24 bits** → solo puede direccionar **16 MB** |
| **80386SL** | Versión para **portátiles** |
| **80376** | Versión **embebida** del 80386SX |
| **80386EX** | Sustituyó al 80376; se utilizó en **aplicaciones espaciales**, como el **Telescopio espacial Hubble** |

### Datos clave del 80386SX

| Parámetro | Valor |
|---|---|
| **Bus de datos** | 16 bits |
| **Bus de direcciones** | 24 bits |
| **Memoria máxima direccionable** | **16 MB** |
| **Bus interno** | 32 bits |

---

## 6. Comparación entre los procesadores

| Característica | 8008 | 8086 | 80286 | 80386DX |
|---|:--:|:--:|:--:|:--:|
| **Año** | 1972 | 1978 | 1982 | 1985 |
| **Bits de la CPU** | 8 | 16 | 16 | 32 |
| **Bus de direcciones** | 14 bits | 20 bits | 24 bits | 32 bits |
| **Memoria máxima** | 16 KB | 1 MB | 16 MB | 4 GB |
| **Velocidad** | 0,5–0,8 MHz | 5–10 MHz | 6–25 MHz | 16–40 MHz |

* La tendencia es clara: con cada generación **aumentan los bits de datos, el bus de direcciones, la memoria direccionable y la velocidad**.

---

## 7. Arquitectura del 8086/8088

* Estos microprocesadores tienen **2 procesadores conectados internamente**:

| Unidad | Sigla | Función |
|---|:--:|---|
| **Unidad de ejecución** | **EU** (*Execution Unit*) | **Ejecuta** las instrucciones: operaciones aritméticas y lógicas, manejo de registros |
| **Unidad de interfaz con el bus** | **BIU** (*Bus Interface Unit*) | **Busca** instrucciones y datos en memoria y gestiona los buses |

* Ambas unidades **trabajan en paralelo**: mientras la EU ejecuta una instrucción, la BIU ya está **leyendo las siguientes** y las guarda en la cola.
* Esta técnica se conoce como ***prefetch* (precarga)** y es un antecedente del *pipeline*.

### 7.1 Unidad de ejecución (EU)

Contiene:

* **Registros de uso general** (de 16 bits).
* **Registros temporales**.
* **Unidad aritmético-lógica (ALU)**.
* **Indicadores** (registro de banderas o *flags*).
* **Sistema de control de la unidad de ejecución**.
* **Bus interno de 16 bits**.

#### Registros de uso general

| Registro | Partes (bits 15-8 / 7-0) | Nombre | Uso típico |
|:--:|:--:|---|---|
| **AX** | AH / AL | Acumulador | Operaciones aritméticas, entrada/salida |
| **BX** | BH / BL | Base | Direccionamiento de datos en memoria |
| **CX** | CH / CL | Contador | Ciclos y repeticiones |
| **DX** | DH / DL | Datos | Multiplicación/división, puertos de E/S |
| **SP** | — | Apuntador de pila (*Stack Pointer*) | Tope de la pila |
| **BP** | — | Apuntador base (*Base Pointer*) | Acceso a datos dentro de la pila |
| **SI** | — | Índice fuente (*Source Index*) | Origen en operaciones con cadenas |
| **DI** | — | Índice destino (*Destination Index*) | Destino en operaciones con cadenas |

* `AX`, `BX`, `CX` y `DX` pueden usarse como **registros de 16 bits** o como **dos registros de 8 bits** (parte alta `H` y parte baja `L`).

#### Indicadores (banderas) del 8086

| Bandera | Significado |
|:--:|---|
| **CF** | Acarreo (*Carry*) |
| **PF** | Paridad |
| **AF** | Acarreo auxiliar |
| **ZF** | Cero |
| **SF** | Signo |
| **OF** | Desbordamiento (*Overflow*) |
| **TF** | Trampa (paso a paso) |
| **IF** | Habilitación de interrupciones |
| **DF** | Dirección |

### 7.2 Unidad de interfaz con el bus (BIU)

Contiene:

* **Registros de segmento:** `CS`, `DS`, `ES`, `SS`.
* **Apuntador de instrucciones:** `IP`.
* **Registros de comunicación interna**.
* **Sumador** (calcula la dirección física de 20 bits).
* **Lógica de control del bus** (conexión con los buses externos).
* **Cola de instrucciones**.

#### Registros de segmento y apuntador de instrucciones

| Registro | Nombre | Función |
|:--:|---|---|
| **CS** | Segmento de código | Segmento donde están las instrucciones del programa |
| **DS** | Segmento de datos | Segmento donde están los datos del programa |
| **SS** | Segmento de pila | Segmento donde está la pila (*stack*) |
| **ES** | Segmento extra | Segmento adicional para datos (por ejemplo, destino de cadenas) |
| **IP** | Apuntador de instrucciones | **Desplazamiento (offset)** de la **siguiente instrucción** dentro del segmento de código |

### 7.3 Buses y cola de instrucciones

| Elemento | 8088 | 8086 |
|---|:--:|:--:|
| **Bus de direcciones** | 20 bits | 20 bits |
| **Bus de datos** | **8 bits** | **16 bits** |
| **Cola de instrucciones** | **4 bytes** | **6 bytes** |

* El **8088** tiene internamente la misma arquitectura de 16 bits del 8086, pero **un bus de datos externo de solo 8 bits** (por eso se usó en el primer IBM PC por su menor costo).
* La **cola de instrucciones** guarda los bytes ya leídos por la BIU para que la EU los tome sin esperar a la memoria.

---

## 8. Mapa de memoria del 8086 (*Memory Map*)

* El bus de direcciones de **20 bits** permite `2^20 = 1 048 576 bytes = 1 MB`, con direcciones desde `00000h` hasta `FFFFFh`.

| Zona | Tamaño | Rango de direcciones (hex) |
|---|:--:|:--:|
| **RAM** | **640 KB** | `00000h` – `9FFFFh` |
| **Video Display RAM** | **128 KB** | `A0000h` – `BFFFFh` |
| **ROM** | **256 KB** | `C0000h` – `FFFFFh` |

```
FFFFFh ┌───────────────────────┐
       │        ROM 256K       │
C0000h ├───────────────────────┤
BFFFFh │                       │
       │  Video Display RAM    │
       │         128K          │
A0000h ├───────────────────────┤
9FFFFh │                       │
       │                       │
       │        RAM 640K       │
       │                       │
00000h └───────────────────────┘
```

### Puntos clave

* El **límite superior de la RAM convencional** es **`9FFFFh`** (en clase se marcó este valor).
* Verificación de tamaños: `A0000h = 655 360 bytes = 640 KB`; `20000h = 131 072 bytes = 128 KB`; `40000h = 262 144 bytes = 256 KB`.
* La **ROM** está en la parte **alta** de la memoria porque, al encenderse, el 8086 comienza a ejecutar en `FFFF0h` (vector de reinicio), donde se ubica el código de arranque (BIOS).
* La memoria de video está entre la RAM y la ROM, por lo que se usa para **mostrar información en pantalla**.

---

## 9. Segmentación y cálculo de la dirección física

* Los registros del 8086 son de **16 bits**, pero el bus de direcciones es de **20 bits**. Para poder direccionar 1 MB se usa la **segmentación de memoria**.
* La memoria se divide en **segmentos** (código `CS`, datos `DS`, pila `SS`, extra `ES`) de hasta **64 KB** cada uno (porque el offset es de 16 bits: `0000h` a `FFFFh`).
* Una dirección se expresa como **`segmento : desplazamiento`**.

### Fórmula

```
Dirección física (20 bits) = (Segmento × 16) + Offset
```

* Multiplicar por 16 equivale a **desplazar el segmento un dígito hexadecimal (4 bits) a la izquierda**, agregando un `0` a la derecha. De esto se encarga el **sumador de la BIU**.

### Ejemplo visto en clase: ejecución de una instrucción desde memoria

* En la CPU: `CS = 1234h` (16 bits) e `IP = 0100h`.

```
CS desplazado  →  12340h
+ IP           →  00100h
-----------------------------
Dirección física = 12440h
```

* El inicio del segmento de código está en **`12340h`** y la instrucción (`Add`, en el dibujo) se encuentra en **`12440h`**.
* En el esquema de memoria se muestran los segmentos **CS, DS, SS y ES**, y el registro `CS` de 16 bits que apunta al segmento de código.
* En la parte superior del dibujo se alcanza a ver una anotación "**+2**" parcialmente cortada, relacionada con el avance de `IP` hacia la siguiente instrucción.
* Tras leer una instrucción, `IP` se **incrementa** según el tamaño de la instrucción, y se repite el cálculo para obtener la siguiente.

### Más ejemplos

| Segmento | Offset | Cálculo | Dirección física |
|:--:|:--:|:--:|:--:|
| `1234h` | `0100h` | `12340h + 0100h` | **`12440h`** |
| `2000h` | `0050h` | `20000h + 0050h` | **`20050h`** |
| `0000h` | `FFFFh` | `00000h + FFFFh` | **`0FFFFh`** (fin del primer segmento de 64 KB) |
| `FFFFh` | `0000h` | `FFFF0h + 0000h` | **`FFFF0h`** (vector de reinicio) |

### Registros usados según el tipo de acceso

| Tipo de acceso | Segmento | Offset |
|---|:--:|:--:|
| **Instrucciones** | `CS` | `IP` |
| **Pila** | `SS` | `SP` / `BP` |
| **Datos** | `DS` | `BX`, `SI` o dirección directa |
| **Cadenas (destino)** | `ES` | `DI` |

* Varios pares `segmento:offset` pueden **apuntar a la misma dirección física** (los segmentos se pueden **traslapar**), ya que los segmentos comienzan cada **16 bytes**.

---

## 10. Resumen general del tema

* El **ábaco** fue la primera calculadora, pero **no es programable**; la computación evolucionó hacia máquinas programables.
* El **Intel 8008** (1972) es de **8 bits**, direcciona **16 KB** (14 bits), trabaja a **0,5–0,8 MHz** y viene en **encapsulado DIP de 18 pines**; fue 3 o 4 veces más rápido que los procesadores de 4 bits.
* El **Intel 80286** (1982) es de **16 bits**, tiene **134 000 transistores**, llega hasta **25 MHz** y direcciona **16 MB** (bus de 24 bits); fue el procesador del **IBM PC/AT**.
* El **Intel 80386** (1985) es de **32 bits**; el **DX** direcciona hasta **4 GB** y trabaja entre **16 y 40 MHz**; el **SX** tiene bus externo de 16 bits y 24 bits de dirección (**16 MB**); existen variantes **SL**, **376** y **EX** (usada en el Hubble).
* El **8086/8088** se compone de dos unidades que trabajan en paralelo: la **EU (ejecución)** y la **BIU (interfaz con el bus)**, comunicadas por una **cola de instrucciones** (4 bytes en el 8088 y 6 bytes en el 8086).
* El **mapa de memoria** del 8086 (1 MB) se divide en **RAM de 640 KB** (`00000h–9FFFFh`), **Video RAM de 128 KB** (`A0000h–BFFFFh`) y **ROM de 256 KB** (`C0000h–FFFFFh`).
* La **dirección física** se calcula como `segmento × 16 + offset`; por ejemplo, `CS = 1234h` e `IP = 0100h` producen la dirección **`12440h`**.

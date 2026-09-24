# =========================================
# MULTIPLEXOR: EJEMPLOS DE DISEÑO APLICADO
# =========================================

> Dividir según cantidad de entradas de MUX; la división sirve para controlar lo enviado a las entradas, la selección es para que las entradas se dividan según binarios para elegir el flujo de los datos que van a transcurrir por el MUX para obtener la salida.
>
> Entradas siempre digitales `2^n` con **1 salida**.

## Idea general del método
- Se parte de la **tabla de verdad completa** de la función.
- Se eligen `n-1` (o menos) variables como **selectoras** (S2, S1, S0...) y se agrupan los renglones de la tabla según esas variables.
- Dentro de cada grupo, el valor de la función se expresa en términos de las **variables residuales** (las que no se usaron como selectoras). Ese valor puede ser: `0` (GND), `1` (VCC), la variable residual, su complemento, o una pequeña expresión booleana de esas variables (si sobra más de una).
- Cada grupo corresponde a **un canal de entrada** del MUX; a cada cuadrante se le realiza su propio Mapa de Karnaugh para obtener la expresión más simple de compuertas, usando la **misma línea de salida** de cada cuadrante como entrada al canal correspondiente.

## Ejemplo 1: MUX 4 a 1 (2 selectores)

**F(A,B,C,D) = Σm(0,2,3,6,7,11,12,15)**

- Selectores: `S2 = A`, `S1 = B` (2 selectores -> MUX 4 a 1).
- Variables residuales: `C`, `D` (sobran 2, por lo que cada entrada del MUX será una función de 2 variables, no solo un bit o una variable simple).

### Tabla de verdad agrupada por AB

| A B (canal) | Minitérminos | C D = 00 | C D = 01 | C D = 10 | C D = 11 | Función resultante |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 00 (canal 0) | m0–m3 | 1 | 0 | 1 | 1 | **C + D'** |
| 01 (canal 1) | m4–m7 | 0 | 0 | 1 | 1 | **C** |
| 10 (canal 2) | m8–m11 | 0 | 0 | 0 | 1 | **C · D** |
| 11 (canal 3) | m12–m15 | 1 | 0 | 0 | 1 | **C XNOR D** |

> Cada función residual se resuelve con un mini Mapa de Karnaugh de 2 variables (C,D) por canal, igual que se haría para cualquier función pequeña.

### Circuito (lógica residual por canal + MUX 4 a 1)

```
Ejemplo 1 (MUX 4 a 1):

              ┌─────┐
           C ─┤     │
              │ OR  ├──── C+D' ────────►┌────────────────┐
          D' ─┤     │                    │0               │
              └─────┘                    │                │
                                          │                │
           C ─────────────────────────────►1              Y├──── F
                                          │                │
              ┌─────┐                    │                │
           C ─┤     │                    │                │
              │ AND ├──── C·D ──────────►│2               │
           D ─┤     │                    │                │
              └─────┘                    │                │
              ┌──────┐                   │                │
           C ─┤      │                   │                │
              │ XNOR ├──── C⊙D ─────────►│3               │
           D ─┤      │                   │   S1   S0      │
              └──────┘                   └────┬────┬──────┘
                                                A    B
```

> **Nota:** aunque el ejemplo original de clase anota el canal 2 como "C + D", al resolver el Mapa K con los minitérminos dados (Σm(0,2,3,6,7,11,12,15)) el resultado correcto es **C · D** (solo es 1 cuando C=1 y D=1, en m11). Los canales 0, 1 y 3 sí coinciden exactamente con lo anotado en clase.

## Ejemplo 2: MUX 2 a 1 (1 selector)

**F(A,B,C,D) = Σm(0,2,3,6,7,11,12,15)** (misma función que el Ejemplo 1)

- Selector: `S1 = A` (1 selector -> MUX 2 a 1).
- Variables residuales: `B`, `C`, `D` (sobran 3, cada entrada será una función de esas 3 variables).

### Tabla de verdad agrupada por A

| A (canal) | Minitérminos | Función resultante en B,C,D |
|:--:|:--:|:--:|
| 0 (canal 0) | m0–m7 | **C + B'D'** |
| 1 (canal 1) | m8–m15 | **CD + BC'D'** |

**Verificación por sub-bloques (usando B como sub-selector interno):**

| A | B | C D = 00 | C D = 01 | C D = 10 | C D = 11 | Subfunción (C,D) |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 0 | 1 | 0 | 1 | 1 | C + D' |
| 0 | 1 | 0 | 0 | 1 | 1 | C |
| 1 | 0 | 0 | 0 | 0 | 1 | C·D |
| 1 | 1 | 1 | 0 | 0 | 1 | C XNOR D |

- Canal 0 = `B'(C+D') + B(C)` = **`C + B'D'`**
- Canal 1 = `B'(CD) + B(C XNOR D)` = **`CD + BC'D'`**

### Circuito (lógica residual por canal + MUX 2 a 1)

```
Ejemplo 2 (MUX 2 a 1):

              ┌─────┐
           C ─┤     │
              │ OR  ├──┐
          B' ─┤     │  ├── C+B'D' ──────►┌────────────────┐
          D' ─┤     │  │                  │0               │
              └─────┘  │                  │               Y├──── F
                (OR 3 entradas)           │                │
              ┌─────┐  │                  │                │
           C ─┤     │  │                  │                │
           D ─┤ AND ├──┤                  │                │
              └─────┘  ├── OR ── CD+BC'D'►│1               │
              ┌─────┐  │                  │       S0       │
           B ─┤     │  │                  └────────┬───────┘
          C' ─┤ AND ├──┘                             A
          D' ─┤     │
              └─────┘
```
---

# =========================================
# CIRCUITOS SECUENCIALES
# =========================================

## Diferencia con los circuitos combinacionales
- En un circuito **combinacional**, la salida depende únicamente de las entradas actuales.
- En un circuito **secuencial**, la salida depende de las entradas actuales **y** de un **estado** anterior (retroalimentación / *feedback*), de ahí el nombre "estado" para diferenciar el cambio que existe entre un momento y otro. Esto estará presente en el *feedback* del curso.

## Flip-Flop
- Es el componente de **memoria** más básico: guarda **1 bit** de información.
- Es un elemento **pasivo**: no cambia su salida por sí solo, solo reacciona ante un **pulso de reloj (CLK)**.
- Requiere un **planeamiento de tiempo (timing)**: el cambio de estado ocurre en un momento preciso relacionado con el reloj, no de forma continua. Este estado se conoce como **estado de excitación**.
- **Borde positivo (rising edge):** transición de 0 a 1 del reloj.
- **Borde negativo (falling edge):** transición de 1 a 0 del reloj.
- Todo componente de memoria tiene una salida **`Q`** y su complemento **`Q'`**. Debe ser **estable**: si `Q=1`, entonces `Q'` debe ser **necesariamente 0** (y viceversa); si ambas coinciden, el estado es **inestable/inválido**.

```
          ┌───────────┐
   D ────►│D         Q├────► Q
          │           │
 CLK ────►│>        Q'├────► Q'
          └───────────┘
```

---

## Flip-Flop D

<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/d2b4a434-7ecd-4c03-b975-3b0692d6d0e0" />

- Recibe un flujo de datos en la entrada **D**.
- En el primer instante `Q = 0` (estado inicial arbitrario, dependiendo del reset).
- **Solo actualiza su salida cuando recibe el borde de reloj** (positivo o negativo, según el diseño): en ese instante, `Q` toma el valor que tenía `D` en ese momento.
- Mientras **no** llega un nuevo pulso de reloj, el flip-flop (al ser pasivo) **no reacciona**, aunque `D` cambie. Solo al llegar el siguiente flanco de reloj, `Q` se actualiza con el valor de `D` vigente **en ese instante**.

### Tabla de característica (Flip-Flop D)

> Se usa `v` (momento actual) y `v+1` (siguiente momento tras el pulso de reloj) en vez de `t` y `t+1`.

| D | Q(v+1) |
|:--:|:--:|
| 0 | 0 — Restablecer (*Reset*) |
| 1 | 1 — Establecer (*Set*) |

```
          ┌───────────┐
   D ────►│D         Q├────► Q(v+1) = D
          │           │
 CLK ────►│>        Q'├────► Q'(v+1) = D'
          └───────────┘
```

---

## Flip-Flop JK

<img width="400" height="248" alt="image" src="https://github.com/user-attachments/assets/12de649c-50de-4dba-bc76-b5c97b8d8c4d" />

<img width="310" height="248" alt="image" src="https://github.com/user-attachments/assets/5515d233-7e88-4358-aee3-28299b12047e" />

- A diferencia del D, tiene **2 entradas de control** (`J` y `K`), por eso se dice que tiene **control**: puede mantener, establecer, restablecer o **complementar** el estado.

```
          ┌───────────┐
   J ────►│J         Q├────► Q
          │           │
 CLK ────►│>          │
          │           │
   K ────►│K        Q'├────► Q'
          └───────────┘
```

### Tabla de característica (Flip-Flop JK)

| J K | Q(v+1) |
|:--:|:--:|
| 0 0 | Q(v) — Sin cambio |
| 0 1 | 0 — Restablecer |
| 1 0 | 1 — Establecer |
| 1 1 | Q'(v) — Complementar |

### Diagramas por combinación (entradas fijas a VCC/GND, salida según CLK)

**JK = 00 (Sin cambio)**
```
 GND ──J──►┌───────────┐
           │J         Q├────► Q(v+1) = Q(v)
 CLK ─────►│>          │
           │           │
 GND ──K──►│K        Q'├────► Q'(v+1) = Q'(v)
           └───────────┘
```
> Aunque llegue el pulso de reloj, la salida no cambia: se queda en su último valor almacenado.

**JK = 01 (Restablecer / Reset)**
```
 GND ──J──►┌───────────┐
           │J         Q├────► Q(v+1) = 0
 CLK ─────►│>          │
           │           │
 VCC ──K──►│K        Q'├────► Q'(v+1) = 1
           └───────────┘
```
> Al llegar el pulso de reloj, `Q` se fuerza a `0` sin importar el valor anterior.

**JK = 10 (Establecer / Set)**
```
 VCC ──J──►┌───────────┐
           │J         Q├────► Q(v+1) = 1
 CLK ─────►│>          │
           │           │
 GND ──K──►│K        Q'├────► Q'(v+1) = 0
           └───────────┘
```
> Al llegar el pulso de reloj, `Q` se fuerza a `1` sin importar el valor anterior.

**JK = 11 (Complementar / Toggle)**
```
 VCC ──J──►┌───────────┐
           │J         Q├────► Q(v+1) = Q'(v)
 CLK ─────►│>          │
           │           │
 VCC ──K──►│K        Q'├────► Q'(v+1) = Q(v)
           └───────────┘
```
> Cada pulso de reloj invierte el estado anterior (comportamiento de "toggle"), igual que un Flip-Flop T con `T=1`.

---

## Flip-Flop T

- Solo tiene **una entrada** (`T`, de *Toggle*). Es un caso particular del JK con `J=K=T`.

```
          ┌───────────┐
   T ────►│T         Q├────► Q(v+1)
          │           │
 CLK ────►│>        Q'├────► Q'(v+1)
          └───────────┘
```

### Tabla de característica (Flip-Flop T)

| T | Q(v+1) |
|:--:|:--:|
| 0 | Q(v) — Sin cambio |
| 1 | Q'(v) — Complementar |

> Aplicación más común de los flip-flops: **contadores**, aprovechando el comportamiento de "complementar" (toggle) para incrementar un valor binario en cada pulso de reloj.

---

## Ejemplo: Contador con Flip-Flop D (0 -> 1 -> 2 -> 3)

- Cuenta en binario: `00 -> 01 -> 10 -> 11 -> 00...` (mayor valor: `11₂ = 3`, contador de **2 bits**).
- Se usan **2 Flip-Flops D**, ambos conectados **al mismo reloj** (`CLK` global) -> es un contador **síncrono**.
- El **LSB (bit menos significativo)** es el que está **más cerca del reloj global** (se conecta directo a `CLK`); el MSB depende de la lógica combinacional derivada del estado del LSB.

### Tabla de transición de estados

| Estado actual (Q1 Q0) | Estado siguiente (Q1' Q0') | D1 (=Q1 siguiente) | D0 (=Q0 siguiente) |
|:--:|:--:|:--:|:--:|
| 00 | 01 | 0 | 1 |
| 01 | 10 | 1 | 0 |
| 10 | 11 | 1 | 1 |
| 11 | 00 | 0 | 0 |

### Ecuaciones de las entradas D

- **D0 = Q0'** (el LSB simplemente se complementa en cada pulso; equivale a un Flip-Flop T con T=1 siempre).
- **D1 = Q1 XOR Q0** (el MSB se complementa únicamente cuando el LSB actual es 1, es decir, cuando "hay acarreo").

### Circuito del contador (2 Flip-Flop D síncronos)

```
    ┌──┤>o│── Q0'
    │
    │              FF0 (LSB)                       FF1 (MSB)
    │            ┌────────────┐                     ┌───────────┐
    └───────────►│D         Q ├──── Q0 ──────┬─────►│           │
                 │            │              │      │           │
    CLK ────┬───►│>         Q'│              │      │    XOR    ├──► D1
            │    └────────────┘              │      │           │
            │                                └─────►│           │
            │                                       └─────┬─────┘
            │                                             │
            │                              ┌───────────┐  │
            │                              │D         Q├──┴──► Q1
            └─────────────────────────────►│>          │
                                           │         Q'│
                                           └───────────┘
```

### Secuencia de conteo

| Pulso CLK | Q1 (MSB) | Q0 (LSB) | Valor decimal |
|:--:|:--:|:--:|:--:|
| Inicio | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 |
| 2 | 1 | 0 | 2 |
| 3 | 1 | 1 | 3 |
| 4 (reinicia) | 0 | 0 | 0 |

> El mismo principio se extiende a contadores de más bits: cada bit adicional se complementa según una condición de "todos los bits anteriores en 1" (acarreo), y todos comparten el mismo reloj en un diseño **síncrono**.





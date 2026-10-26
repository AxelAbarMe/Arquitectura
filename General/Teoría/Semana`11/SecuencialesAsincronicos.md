# Flip-Flop JK asíncrono: contadores asíncronos (MOD, conexiones y tablas)

## 1. Repaso del Flip-Flop JK en modo toggle

* Con `J = K = 1` el flip-flop **complementa su salida** en cada borde activo de su reloj.

| J K | Q(v+1) | Operación |
|:--:|:--:|:--|
| 0 0 | Q(v) | Sin cambio |
| 0 1 | 0 | Restablecer |
| 1 0 | 1 | Establecer |
| 1 1 | Q'(v) | Complementar (toggle) |

* En modo toggle, **cada flip-flop divide la frecuencia de su reloj entre 2**: su salida `Q` tiene la mitad de la frecuencia de su entrada `CLK`.

---

## 2. ¿Qué es un contador asíncrono?

* También se llama contador de **rizado** (*ripple counter*) o de **propagación**.
* **Solo el primer flip-flop (LSB) recibe el reloj externo.** Cada flip-flop siguiente recibe como reloj **la salida del anterior**.
* Los flip-flops **no cambian al mismo tiempo**: el cambio se **propaga en cadena** (cada uno espera al anterior).
* Todos los flip-flops trabajan con `J = K = 1` (conectados a VCC).

```
          J=K=1             J=K=1             J=K=1
        ┌────────┐        ┌────────┐        ┌────────┐
CLK ───►│>  FF0 Q├─ Q0 ──►│>  FF1 Q├─ Q1 ──►│>  FF2 Q├─ Q2
        └────────┘        └────────┘        └────────┘
         (LSB)                                 (MSB)
```

---

## 3. Regla de conexión según el tipo de flanco

| Flip-flop activado por | Conexión al siguiente `CLK` | Tipo de cuenta |
|---|---|---|
| **Flanco negativo** (↓) | `Q` del anterior | **Ascendente** |
| **Flanco negativo** (↓) | `Q'` del anterior | **Descendente** |
| **Flanco positivo** (↑) | `Q'` del anterior | **Ascendente** |
| **Flanco positivo** (↑) | `Q` del anterior | **Descendente** |

* **Idea clave:** el siguiente flip-flop debe cambiar cuando el bit anterior **pasa de 1 a 0** (hay acarreo) en la cuenta ascendente.
* **Número de flip-flops:** `n = ⌈log₂ N⌉`.

---

## 4. Contador asíncrono ascendente MOD-2^n

### 4.1 MOD-8 (3 flip-flops, flanco negativo, `CLK(i+1) = Qi`)

| Pulso CLK | Q2 | Q1 | Q0 | Decimal |
|:--:|:--:|:--:|:--:|:--:|
| 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 1 | 1 |
| 2 | 0 | 1 | 0 | 2 |
| 3 | 0 | 1 | 1 | 3 |
| 4 | 1 | 0 | 0 | 4 |
| 5 | 1 | 0 | 1 | 5 |
| 6 | 1 | 1 | 0 | 6 |
| 7 | 1 | 1 | 1 | 7 |
| 8 | 0 | 0 | 0 | 0 (reinicia) |

### Diagrama de tiempos (simplificado)

```
CLK  ┐┌┐┌┐┌┐┌┐┌┐┌┐┌┐┌┐
Q0   ┐ ┌─┐ ┌─┐ ┌─┐ ┌─     (cambia en cada ↓ de CLK)
Q1   ┐   ┌───┐   ┌───     (cambia en cada ↓ de Q0)
Q2   ┐       ┌───────     (cambia en cada ↓ de Q1)
```

### 4.2 Frecuencias

```
f(Q0) = f_CLK / 2
f(Q1) = f_CLK / 4
f(Q2) = f_CLK / 8
f(Q(n-1)) = f_CLK / 2^n
```

### 4.3 MOD-16 (4 flip-flops)

| Pulso | Q3 Q2 Q1 Q0 | Decimal | Pulso | Q3 Q2 Q1 Q0 | Decimal |
|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 0000 | 0 | 8 | 1000 | 8 |
| 1 | 0001 | 1 | 9 | 1001 | 9 |
| 2 | 0010 | 2 | 10 | 1010 | 10 |
| 3 | 0011 | 3 | 11 | 1011 | 11 |
| 4 | 0100 | 4 | 12 | 1100 | 12 |
| 5 | 0101 | 5 | 13 | 1101 | 13 |
| 6 | 0110 | 6 | 14 | 1110 | 14 |
| 7 | 0111 | 7 | 15 | 1111 | 15 |

* Tras el estado `1111` el siguiente pulso reinicia el conteo a `0000`.

---

## 5. Contador asíncrono descendente

* Con flip-flops de **flanco negativo** se conecta `Q'` del anterior al `CLK` del siguiente.

### MOD-8 descendente

| Pulso CLK | Q2 | Q1 | Q0 | Decimal |
|:--:|:--:|:--:|:--:|:--:|
| 0 | 1 | 1 | 1 | 7 |
| 1 | 1 | 1 | 0 | 6 |
| 2 | 1 | 0 | 1 | 5 |
| 3 | 1 | 0 | 0 | 4 |
| 4 | 0 | 1 | 1 | 3 |
| 5 | 0 | 1 | 0 | 2 |
| 6 | 0 | 0 | 1 | 1 |
| 7 | 0 | 0 | 0 | 0 |
| 8 | 1 | 1 | 1 | 7 (reinicia) |

---

## 6. Contador asíncrono MOD-N (N no es potencia de 2)

* Se construye con `n = ⌈log₂ N⌉` flip-flops y se **trunca** la cuenta.
* Se usa una compuerta **NAND** que **detecta el estado `N`** (el primer estado que no debe existir) y reinicia todos los flip-flops mediante la entrada asíncrona **`CLR`** (activa en bajo).

### Procedimiento

1. Calcular `n = ⌈log₂ N⌉` y construir el contador ascendente básico.
2. Escribir el número `N` en binario (es el **estado de reinicio**).
3. Conectar a una **NAND** las salidas `Q` que valen **1** en ese estado (basta con las necesarias para identificarlo de forma única).
4. Conectar la salida de la NAND a las entradas **`CLR`** de **todos** los flip-flops.
5. Cuando el contador llega al estado `N`, la NAND pone `CLR = 0` y el contador vuelve a `0` casi de inmediato.

### Tabla de contadores MOD-N más comunes

| MOD | Flip-flops | Estado de reinicio (N) | Salidas a la NAND |
|:--:|:--:|:--:|:--|
| **MOD-3** | 2 | 11 | `Q1 · Q0` |
| **MOD-5** | 3 | 101 | `Q2 · Q0` |
| **MOD-6** | 3 | 110 | `Q2 · Q1` |
| **MOD-7** | 3 | 111 | `Q2 · Q1 · Q0` |
| **MOD-9** | 4 | 1001 | `Q3 · Q0` |
| **MOD-10** | 4 | 1010 | `Q3 · Q1` |
| **MOD-11** | 4 | 1011 | `Q3 · Q1 · Q0` |
| **MOD-12** | 4 | 1100 | `Q3 · Q2` |
| **MOD-13** | 4 | 1101 | `Q3 · Q2 · Q0` |
| **MOD-14** | 4 | 1110 | `Q3 · Q2 · Q1` |
| **MOD-15** | 4 | 1111 | `Q3 · Q2 · Q1 · Q0` |

### Conexión del MOD-6 (3 flip-flops + NAND)

```
          J=K=1             J=K=1             J=K=1
        ┌────────┐        ┌────────┐        ┌────────┐
CLK ───►│>  FF0 Q├─ Q0 ──►│>  FF1 Q├─ Q1 ──►│>  FF2 Q├─ Q2
        │     CLR│        │     CLR│        │     CLR│
        └───▲────┘        └───▲────┘        └───▲────┘
            └─────────────────┴─────────────────┘
                              │
                         ┌────┴────┐
              Q1 ───────►│  NAND   │
              Q2 ───────►│         │
                         └─────────┘
```

### Secuencia del MOD-6

| Pulso | Q2 Q1 Q0 | Decimal | Observación |
|:--:|:--:|:--:|:--|
| 0 | 000 | 0 | |
| 1 | 001 | 1 | |
| 2 | 010 | 2 | |
| 3 | 011 | 3 | |
| 4 | 100 | 4 | |
| 5 | 101 | 5 | |
| 6 | 110 | (6) | Estado **transitorio**: la NAND lo detecta y activa `CLR` |
| 6 | 000 | 0 | El contador ya reinició |

### Secuencia del MOD-10 (década, 4 flip-flops, NAND con `Q3·Q1`)

| Pulso | Q3 Q2 Q1 Q0 | Decimal |
|:--:|:--:|:--:|
| 0 | 0000 | 0 |
| 1 | 0001 | 1 |
| 2 | 0010 | 2 |
| 3 | 0011 | 3 |
| 4 | 0100 | 4 |
| 5 | 0101 | 5 |
| 6 | 0110 | 6 |
| 7 | 0111 | 7 |
| 8 | 1000 | 8 |
| 9 | 1001 | 9 |
| 10 | 1010 | (10) transitorio → reinicia a 0000 |

* **Observación:** el estado `N` existe solo durante un instante muy corto (**pulso de reinicio**, *glitch*); por eso en una salida decodificada puede aparecer un pulso espurio.

---

## 7. Entradas asíncronas PRESET y CLEAR

* Son entradas que **actúan sin esperar el reloj** y tienen **prioridad** sobre `J`, `K` y `CLK`.
* En la mayoría de los circuitos integrados son **activas en bajo** (`PRE'` y `CLR'`).

| PRE' | CLR' | Q | Operación |
|:--:|:--:|:--:|:--|
| 0 | 1 | 1 | **Preset**: fuerza `Q = 1` |
| 1 | 0 | 0 | **Clear**: fuerza `Q = 0` |
| 1 | 1 | Según J, K y CLK | Operación normal |
| 0 | 0 | No permitido | Estado inválido (`Q` y `Q'` coinciden) |

---

## 8. Cuenta con secuencia no estándar (aleatoria) en un asíncrono

* Se puede **forzar un salto** de estado con **PRESET/CLEAR** al detectar un estado determinado.
* Ejemplo: **cuenta de 3 a 7** (`011 → 100 → 101 → 110 → 111 → 011 …`) con 3 flip-flops de flanco negativo:
  1. Cuando el contador llega a `000` (tras `111`), una **NAND con `Q2'·Q1'·Q0'`** activa `PRE'` en `Q1` y `Q0`.
  2. Estas dos salidas pasan a `1` y el contador queda en `011`.
* Limitaciones: la cuenta depende de **pulsos muy cortos** y del **retardo de propagación**, por lo que no es confiable a alta frecuencia.
* Para secuencias arbitrarias complejas se recomienda un **diseño síncrono**.

---

## 9. Ventajas y desventajas del contador asíncrono

| Ventajas | Desventajas |
|---|---|
| Circuito **simple**: solo flip-flops con `J = K = 1` | **Retardo acumulado**: cada flip-flop espera al anterior |
| **Pocos componentes** | Velocidad limitada: `f_max ≈ 1 / (n · t_pd)` |
| Funciona como **divisor de frecuencia** (÷2^n) | Estados **intermedios erróneos** durante la propagación (*glitches*) |
| Fácil de ampliar agregando flip-flops | Las salidas **no cambian simultáneamente** |
| Fácil de truncar a MOD-N con NAND y `CLR` | El estado `N` aparece brevemente antes del reinicio |

### Ejemplo del retardo acumulado (MOD-8, ↓ en 111 → 000)

```
CLK ↓ → Q0: 1→0 → Q1: 1→0 → Q2: 1→0
         t_pd        2·t_pd     3·t_pd
Estados intermedios: 111 → 110 → 100 → 000
```

---

## 10. Comparación: asíncrono vs. síncrono

| Aspecto | Asíncrono (rizado) | Síncrono |
|---|---|---|
| **Reloj** | Solo el primer flip-flop; los demás usan la salida del anterior | Común para todos |
| **Entradas J y K** | `J = K = 1` (fijas) | Dependen del estado (lógica combinacional) |
| **Cambio de salidas** | En cadena (propagación) | Simultáneo |
| **Velocidad** | Menor (retardo acumulado) | Mayor |
| **Complejidad del diseño** | Baja | Media/alta |
| **Truncado a MOD-N** | NAND + `CLR` | Tabla de excitación y Karnaugh |
| **Secuencias arbitrarias** | Poco práctico | Diseño directo |
| **Glitches** | Frecuentes | Pocos |

---

## 11. Resumen general

* En un contador asíncrono **solo el primer flip-flop recibe el reloj externo**; los demás se activan con la salida del anterior (`Q` o `Q'` según el flanco y el sentido de la cuenta).
* Todos los flip-flops usan `J = K = 1` (modo toggle) y **cada uno divide la frecuencia entre 2**.
* Un contador de `n` flip-flops tiene **MOD = 2^n** y divide la frecuencia del reloj entre `2^n`.
* Para un **MOD-N** (N no potencia de 2) se usan `n = ⌈log₂ N⌉` flip-flops y una **NAND** que detecta el estado `N` y activa **`CLR`** de todos los flip-flops.
* `PRESET` y `CLEAR` son entradas **asíncronas** con **prioridad** sobre el reloj.
* La principal desventaja es el **retardo acumulado** y los **estados transitorios**; su ventaja es la **simplicidad**.

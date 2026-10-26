# Flip-Flop JK síncrono: contadores síncronos (no aleatorios y aleatorios)

## 1. Repaso del Flip-Flop JK

* Es un elemento de **memoria de 1 bit** con **dos entradas de control** (`J` y `K`) y una entrada de reloj (`CLK`).
* Es **pasivo**: solo actualiza su salida en el **borde de reloj** (positivo o negativo, según el diseño).
* Se usa `v` (momento actual) y `v+1` (siguiente momento tras el pulso de reloj).

### Tabla de característica

| J K | Q(v+1) | Operación |
|:--:|:--:|:--|
| 0 0 | Q(v) | Sin cambio (mantiene) |
| 0 1 | 0 | Restablecer (*Reset*) |
| 1 0 | 1 | Establecer (*Set*) |
| 1 1 | Q'(v) | Complementar (*Toggle*) |

### Ecuación característica

```
Q(v+1) = J·Q'(v) + K'·Q(v)
```

### Tabla de excitación (la que se usa para diseñar)

* Indica **qué valores deben tener J y K** para lograr una transición deseada `Q(v) → Q(v+1)`.

| Q(v) → Q(v+1) | J | K | Razón |
|:--:|:--:|:--:|:--|
| 0 → 0 | 0 | X | Mantener (J=0,K=0) o restablecer (J=0,K=1) |
| 0 → 1 | 1 | X | Establecer (J=1,K=0) o complementar (J=1,K=1) |
| 1 → 0 | X | 1 | Restablecer (J=0,K=1) o complementar (J=1,K=1) |
| 1 → 1 | X | 0 | Mantener (J=0,K=0) o establecer (J=1,K=0) |

* `X` significa **indiferente** (*don't care*): puede valer 0 o 1 y se aprovecha en los Mapas de Karnaugh para simplificar.

---

## 2. ¿Qué es un contador síncrono?

* Un **contador** es un circuito secuencial formado por flip-flops que recorre una **secuencia de estados** con cada pulso de reloj.
* En un contador **síncrono**, **todos los flip-flops comparten el mismo reloj (`CLK` global)**: cambian **al mismo tiempo**.
* Cada flip-flop decide si cambia o no mediante la **lógica combinacional** conectada a sus entradas `J` y `K`, que depende del **estado actual**.
* El número de flip-flops `n` se obtiene con `2^n ≥ cantidad de estados` (es decir, `n = ⌈log₂ N⌉`).

| Característica | Contador síncrono |
|---|---|
| **Reloj** | Común para todos los flip-flops |
| **Cambio de salidas** | Simultáneo (en el mismo flanco) |
| **Retardo** | Solo el de **un** flip-flop más el de la compuerta AND |
| **Velocidad máxima** | Alta: `f_max ≈ 1 / (t_FF + t_AND + t_setup)` |
| **Diseño** | Más complejo (requiere lógica en J y K) |
| **Glitches** | Pocos o ninguno en las salidas |

---

## 3. Procedimiento general de diseño

1. Definir la **secuencia de estados** (diagrama de estados) y la cantidad de estados `N`.
2. Calcular el número de flip-flops `n = ⌈log₂ N⌉`.
3. Construir la **tabla de estados**: estado presente `Q(v)` → estado siguiente `Q(v+1)`.
4. Con la **tabla de excitación del JK**, obtener los valores de `J` y `K` de cada flip-flop para cada transición.
5. Marcar los **estados no usados** como `X` (indiferentes) en todas las entradas.
6. Simplificar cada `J` y `K` con **Mapas de Karnaugh**.
7. Dibujar el circuito: todos los `CLK` unidos, y cada `J`/`K` conectado a su expresión.
8. **Verificar los estados no usados**: comprobar que el contador regresa solo a la secuencia válida (*auto-corrección*).

---

## 4. Contadores NO aleatorios (secuencia binaria)

* Siguen la **cuenta binaria natural** (ascendente o descendente). Presentan un **patrón fijo**: cada bit se complementa cuando **todos los bits anteriores** están en el valor adecuado.
* Cada flip-flop trabaja en modo **toggle** (`J = K`) cuando le toca cambiar y en modo **mantener** (`J = K = 0`) cuando no.

### 4.1 Contador ascendente de 2 bits (MOD-4)

| Q1 Q0 (v) | Q1 Q0 (v+1) | J1 K1 | J0 K0 |
|:--:|:--:|:--:|:--:|
| 00 | 01 | 0 X | 1 X |
| 01 | 10 | 1 X | X 1 |
| 10 | 11 | X 0 | 1 X |
| 11 | 00 | X 1 | X 1 |

```
J0 = K0 = 1
J1 = K1 = Q0
```

### 4.2 Contador ascendente de 3 bits (MOD-8)

| Q2 Q1 Q0 (v) | Q2 Q1 Q0 (v+1) | J2 K2 | J1 K1 | J0 K0 |
|:--:|:--:|:--:|:--:|:--:|
| 000 | 001 | 0 X | 0 X | 1 X |
| 001 | 010 | 0 X | 1 X | X 1 |
| 010 | 011 | 0 X | X 0 | 1 X |
| 011 | 100 | 1 X | X 1 | X 1 |
| 100 | 101 | X 0 | 0 X | 1 X |
| 101 | 110 | X 0 | 1 X | X 1 |
| 110 | 111 | X 0 | X 0 | 1 X |
| 111 | 000 | X 1 | X 1 | X 1 |

```
J0 = K0 = 1
J1 = K1 = Q0
J2 = K2 = Q1 · Q0
```

### 4.3 Contador ascendente de n bits (patrón general)

```
J0 = K0 = 1
J1 = K1 = Q0
J2 = K2 = Q1 · Q0
J3 = K3 = Q2 · Q1 · Q0
...
Jn = Kn = Q(n-1) · ... · Q1 · Q0
```

* **Regla:** un bit se complementa **solo cuando todos los bits menos significativos valen 1** (hay "acarreo").
* El `LSB` siempre se complementa (`J = K = 1`).

### 4.4 Contador descendente

* Se usa el mismo patrón, pero con las salidas **complementadas** (`Q'`): un bit se complementa cuando todos los bits menos significativos valen 0.

```
J0 = K0 = 1
J1 = K1 = Q0'
J2 = K2 = Q1' · Q0'
J3 = K3 = Q2' · Q1' · Q0'
```

### 4.5 Contador ascendente/descendente (con línea de control M)

* `M = 1` → cuenta **ascendente**; `M = 0` → cuenta **descendente**.

```
J0 = K0 = 1
J1 = K1 = M·Q0 + M'·Q0'
J2 = K2 = M·Q1·Q0 + M'·Q1'·Q0'
```

### 4.6 Contador síncrono MOD-N truncado (cuenta que no usa todos los estados)

* Se usa cuando `N` **no es potencia de 2**. Se recorren los estados `0 … N−1` y luego se regresa a `0`.
* Los estados sobrantes (`N … 2^n − 1`) se marcan como **indiferentes (X)**.

#### Ejemplo: contador MOD-6 (0 → 1 → 2 → 3 → 4 → 5 → 0), 3 flip-flops

| Q2 Q1 Q0 (v) | Q2 Q1 Q0 (v+1) | J2 K2 | J1 K1 | J0 K0 |
|:--:|:--:|:--:|:--:|:--:|
| 000 | 001 | 0 X | 0 X | 1 X |
| 001 | 010 | 0 X | 1 X | X 1 |
| 010 | 011 | 0 X | X 0 | 1 X |
| 011 | 100 | 1 X | X 1 | X 1 |
| 100 | 101 | X 0 | 0 X | 1 X |
| 101 | 000 | X 1 | 0 X | X 1 |
| 110 | — | X X | X X | X X |
| 111 | — | X X | X X | X X |

**Ecuaciones (tras simplificar con Mapas de Karnaugh):**

```
J0 = 1        K0 = 1
J1 = Q2'·Q0   K1 = Q0
J2 = Q1·Q0    K2 = Q0
```

**Verificación de estados no usados (auto-corrección):**

| Estado no usado | Siguiente estado | Resultado |
|:--:|:--:|:--|
| 110 | 111 | Pasa a otro estado no usado |
| 111 | 000 | Regresa a la secuencia válida |

#### Ejemplo: contador de década BCD MOD-10 (0 → 9 → 0), 4 flip-flops

```
J0 = K0 = 1
J1 = Q3'·Q0     K1 = Q0
J2 = Q1·Q0      K2 = Q1·Q0
J3 = Q2·Q1·Q0   K3 = Q0
```

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
| 10 | 0000 | 0 (reinicia) |

* **Estados no usados (10 a 15):** 1010→1011→0101, 1100→1101→0100, 1110→1111→0000. Todos regresan a la secuencia válida.

---

## 5. Contadores ALEATORIOS (secuencia arbitraria)

* Siguen una **secuencia de estados definida por el diseñador**, que **no es la cuenta binaria natural** (ej. `0 → 2 → 5 → 3 → 7 → 0`).
* No existe un patrón general: **cada flip-flop se diseña por separado** con su propia tabla de excitación y su Mapa de Karnaugh.
* Los estados que no pertenecen a la secuencia son **indiferentes (X)**, lo que permite simplificar mucho las expresiones.

### Ejemplo: secuencia 0 → 2 → 5 → 3 → 7 → 0 (3 flip-flops)

* Estados no usados: `001`, `100`, `110`.

| Q2 Q1 Q0 (v) | Q2 Q1 Q0 (v+1) | J2 K2 | J1 K1 | J0 K0 |
|:--:|:--:|:--:|:--:|:--:|
| 000 | 010 | 0 X | 1 X | 0 X |
| 010 | 101 | 1 X | X 1 | 1 X |
| 101 | 011 | X 1 | 1 X | X 0 |
| 011 | 111 | 1 X | X 0 | X 0 |
| 111 | 000 | X 1 | X 1 | X 1 |
| 001 | — | X X | X X | X X |
| 100 | — | X X | X X | X X |
| 110 | — | X X | X X | X X |

**Ecuaciones (tras simplificar):**

```
J2 = Q1          K2 = 1
J1 = 1           K1 = Q2 + Q0'
J0 = Q1          K0 = Q2·Q1
```

**Verificación de estados no usados:**

| Estado no usado | Siguiente estado | Resultado |
|:--:|:--:|:--|
| 001 | 011 | Entra a la secuencia |
| 100 | 010 | Entra a la secuencia |
| 110 | 001 | Pasa a 001 y luego a 011 |

* Si algún estado no usado **no regresara** a la secuencia, se debe **modificar el diseño** (asignando valores a las `X` de ese estado) para evitar el **bloqueo** (*lock-out*).

---

## 6. Comparación: no aleatorio vs. aleatorio

| Aspecto | No aleatorio (binario) | Aleatorio (arbitrario) |
|---|---|---|
| **Secuencia** | Cuenta binaria natural | Definida por el diseñador |
| **Patrón de J y K** | Fijo y general (`Q(n-1)···Q0`) | Distinto en cada flip-flop |
| **Método** | Fórmula directa | Tabla de excitación + Karnaugh |
| **Estados sobrantes** | Ninguno (MOD-2^n) o `X` (MOD-N) | `X` y requieren verificación |
| **Complejidad** | Baja | Media/alta |

---

## 7. Resumen general

* Un contador **síncrono** conecta **todos los `CLK` al mismo reloj**; las salidas cambian **al mismo tiempo**.
* La **tabla de excitación del JK** (`0→0: 0X`, `0→1: 1X`, `1→0: X1`, `1→1: X0`) es la herramienta central del diseño.
* En los contadores **no aleatorios**, `Ji = Ki = Q(i-1)···Q0` (ascendente) o con `Q'` (descendente).
* En los contadores **MOD-N** se usan los estados sobrantes como `X` y se verifica la **auto-corrección**.
* En los contadores **aleatorios** cada flip-flop se resuelve con su propia tabla y Karnaugh.
* La principal ventaja frente al asíncrono es su **mayor velocidad** y la **ausencia de retardo acumulado**.

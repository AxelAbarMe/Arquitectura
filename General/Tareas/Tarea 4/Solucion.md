# TAREA 4

## 1. Ejemplo de Examen
Implemente -de manera óptima y mínima- el circuito lógico de la F. (Sólo con compuertas de NAND)

- d(D,C,B,A) = Σ(4,5,10,11,12)
- F(D,C,B,A) = Σ(0,7,8,13,4)

## 2. Obtenga / Simplifique e implemente la función

F(A,B,C,D) = A'B'C' + B'CD' + A'BCD' + AB'C'

## 3. Obtenga / Simplifique e implemente las siguientes funciones

- F(x, y, z) = Σ(1, 3, 7)
- F(x, y, z) = Σ(0, 1, 2, 6)
- F(a, b, c) = Σ(0, 1, 2, 3, 4, 5)
- F(w, x, y, z) = Σ(0, 1, 2, 4, 5, 6, 8, 9, 12, 13, 14)
- F = A'B'C' + B'CD' + A'BCD' + AB'C'

### Implementa el circuito lógico utilizando los siguientes con compuertas de NAND:

**Mapa K #3** (CD\AB)

| CD \ AB | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 1 | 0 | 0 | 1 |
| **01** | 1 | 1 | 1 | 1 |
| **11** | 1 | 0 | 0 | 1 |
| **10** | 1 | 0 | 0 | 1 |

**Mapa K #4** (Y / AB, con CD)

| Y \ AB | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **CD 00** | 1 | 0 | 0 | 0 |
| **CD 01** | 1 | 1 | 1 | 0 |
| **CD 11** | 1 | 1 | 0 | 1 |
| **CD 10** | 1 | 0 | 0 | 0 |

### Implementa el circuito lógico utilizando los siguientes con compuertas de NAND:

**Mapa K #1** (CD\AB)

| CD \ AB | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 1 |  | 1 | 1 |
| **01** |  | 1 | 1 | 1 |
| **11** |  | 1 | 1 |  |
| **10** | 1 |  | X | 1 |

**Mapa K #2** (CD\AB)

| CD \ AB | 00 | 01 | 11 | 10 |
| --- | --- | --- | --- | --- |
| **00** | 1 |  |  | 1 |
| **01** | 1 | 1 | 1 | 1 |
| **11** | 1 | 1 | 1 | 1 |
| **10** | 1 |  | X | 1 |

---



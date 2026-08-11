> Nota: Velocidad de internet medido en bits por segundo, 2^x.

# Código ASCII

> Explicación práctica de ley de Ohm: Si tiene una resistencia, cuando pasa corriente genera voltaje. Los 8bit para cada tecla es diferente. Computacionales son sistema digital, basado en 4bit, 8bit, 16bit. Osea una combinación de 2^x. Estándar existe para que todo hable el mismo idioma.

## Tabla de ejemplo

> Se toma los valores de arriba para b7b6b5, y los valores de la izquierda para el resto de bits.

<img src="../../../img/ASCII_Table.png" Alt="ASCII" width="400">

> Bit de paridad (Detección de errores) (Bit de signo) (MSB)

|b7|b6|b5|b4|b3|b2|b1|Resultado
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|1|0|0|0|0|0|1|A
|1|0|0|0|0|0|0|@

### Ejemplo Bin a Dec

1010 1000

10x16 8  => 168

> Se toma los segmentos de 4 bits y se obtiene su valor decimal y se multiplica por 16^x, luego se suma. Donde x es su posición del bloque.

Protocolo de envió: Paridad

- Par: Contar cantidades de 1 debe ser par, resultado 0, sino 1.
- Impar: Contar cantidades de 1 debe ser impar, resultado 0, sino 1.

Los 8bits deben de tener la paridad indicada, en base a lo que se indique si este es par o impar.

## Ejemplo Tarea

- Par   | [1] 1 1 0 0 0 0 1 | a  | Paridad es 1 debido a que ya que hay una cantidad impar de 1, error de paridad

- Impar | [0] 1 1 0 0 0 1 0 | b  | Paridad es 0 debido a que ya hay una cantidad impar de 1

# Algebra booleana

- Simular: Imitación, puro software.
- Emular: Si utiliza recursos CPU es emular.

Ventaja de reducir cantidad de componentes electrónicos

---

<img src="../../../img/Algebra_Bool.png" Alt="Boolean Algebra" width="600">

AND: xy

OR: x+y

- VCC
- Ground

Diferencia entre x + 0 = x, letargo o delay

Prima de 5V es tierra, x con un inversor es x'.

14 Pines, VCC siempre conectado a 5V. Chip debe tener voltaje de referencia 0, lo que genera corriente es la diferencia de voltaje.

### Como es posible que una NAND que recibe entradas 0|0 genere 1, osea energía.

Lo que sucede es que el transitor se enciende cuando VCC provee 5V, lo que provoca que se reste un voltaje similar provocando que de entrada 1 genere 0, pero cuando VCC provee 0V el transistor no se enciende, lo que provoca ahora que el voltaje que se debió restar se mantenga igual y este si se envié el voltaje igual.

Se ocupa tierra para cerrar el circuito.

Sistemas digitales están diseñados para bajo voltaje y baja corriente.

# Digitalwork

Half Adder
















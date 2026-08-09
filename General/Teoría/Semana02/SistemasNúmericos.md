# Sistemas Binarios
- Definición de sistema digital: Conjunto de elementos digitales o electrónicos para cumplir una función con entradas y salidas.


## Sistema digital vs Sistema analógico


Sistema digital utilizan 1s y 0s. Un significado matemático, pero a nivel físico significa 5V (3.3V dependiendo del sistema) o 0V, osea apagado o prendido.


Sistema analógico para cada x existe un y.


Digital tiene frecuencia 0, datos trabajados son 0 y 1.
Analógico frecuencia frecuente 110Hz


Sistema digital presenta inmunidad a interferencias que en el analógico si están presente.


Un bit es el espacio más pequeño de la memoria.
Un byte es 8 bit.
Ancho de banda al medir Megabit, megabyte, terabyte son tamaños.


## Sistemas númericos:
- Binario
- Hexadecimal
- Octal


| Bin | Hex | Dec |
|:---:|:----:|:---:
|0000|0|0|
|0001|1|1|
|0010|2|2|
|0011|3|3|
|0100|4|4|
|0101|5|5|
|0110|6|6|
|0111|7|7|
|1000|8|8|
|1001|9|9|
|1010|A|10|
|1011|B|11|
|1100|C|12|
|1101|D|13|
|1110|E|14|
|1111|F|15|


Procesador maneja mejor el flujo de datos utilizando hexadecimal antes que binario al manejar registros y memorias internas.


### Recomendación: w3Schools.com


## Conversion entre bases


### Decimal a Bin/Hex


#### Técnica División


Dividir número decimal por la base 2 o base 16 hasta que el cociente sea menor a divisor.


- MSB ( Most Significant bit)
- LSB ( Less Significant bit)


#### Ejemplo:


Div |20/2 | 10/2 | 5/2 | 2/2 | 1
|:---:|:---:|:---:|:---:|:---:|:---:|
Res | 0    |  0  |    1   |  0|


Bin = 10100


Múltiples divisiones:


- Hardware -> Complejidad
- Software -> Tiempo


---


### Proceso inverso: Bin/Hex -> Decimal


#### Expandir la base Bin


|2^3|2^2|2^1|2^0
|:---:|:---:|:---:|:---:|
|1|0|1|1
|1*2^3| 0*2^2 | 1*2^1 | 1*2^0|


## Imagen de ejemplo Proceso


<img src="../../img/Bin-Dec.png">


#### Hexadecimal


|16^1|16^0
|:---:|:---:|
|F|A
| 15*16^1 | 10*16^0|


## Imagen de ejemplo Proceso


<img src="https://www.wikihow.com/images_en/thumb/0/0d/1797961-8-1.jpg/v4-460px-1797961-8-1.jpg">


### En caso fracción 250.38


## 250.38 Bin
|2^7|2^6|2^5|2^4|2^3|2^2|2^1|2^0|2^-1|2^-2|2^-3
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1|1|1|1|1|0|1|0|0|1|1


250 = 11111010


Se obtiene el decimal y se realiza, 0.38 x 2 = 0.76, se agarra el digito entero


Luego se obtiene 0.76 x 2 = 1.52, se agarra el 1.


Nuevamente se obtiene 0.52 x 2 = 1.04, se agarra el 1 nuevamente y así sucesivamente.


## 250.38 Hex
|16^1|16^0
|:---:|:---:|
|F|A


Para obtener los decimales, se obtienen y se múltiplican por la base, osea 0.38 x 16 = 6.08. Se reserva entonces el decimal 6.


FA.6


Se continua entonces con 0.08 x 16 = 1.28, se reserva entonces el 1.


FA.61


Se continua entonces con 0.28 x 16 = 480 - 32 = 4.48. Se reserva el 4


FA.614

## Bin a Hex

<img src="https://www.wikihow.com/images/1/17/Convert-Binary-to-Hexadecimal-Step-11-Version-2.jpg" width="450">

Se dividen en conjuntos de 4 bits, que corresponden a un valor hexadecimal

|Ent|-|Dec
|:---:|:---:|:---:|
|110|.|11
|0110|.|1100
|6|.|C










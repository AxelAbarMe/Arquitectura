# Multiplexor

> Dividir según cantidad de entradas de MUX, la división sirve para controlar lo enviado a las entradas, la selección es para que las entradas s dividan según binarios para elegir el flujo de los binarios que van a transcurrir por el MUX para obtener la salida
>
> Entradas siempre digitales 2^n con 1 salida

## Ejemplo 1

**F(A,B,C,D) = Σm(0,2,3,6,7,11,12,15)**

Tabla de verdad, división según cantidad de las entradas, s2 y s1 al usar MUX de 4-1. y realizar mapa K de cada cuadrante para obtener compuertas para multiplexor, utilizar misma salida de cable para todas las entradas.

MUX

* 0: C + D`
* 1: C
* 2: C + D
* 3: C XNOR D

* s2: A
* s1: B

## Ejemplo 2

**F(A,B,C,D) = Σm(0,2,3,6,7,11,12,15)**

Tabla de verdad, división según cantidad de las entradas, s1 al usar MUX de 2-1. y realizar mapa K de cada cuadrante para obtener compuertas para multiplexor, utilizar misma salida de cable para todas las entradas.

MUX

* 0: C  + B\`D`
* 1: CD + BD\`C`

* s1: A

# Circuitos secuenciales

Estarán en el feedback, se llama estado para saber diferenciar el cambio que existe.

Flip Flop es un componente que guarda un bit de información

Pulso de reloj en circuito secuencial, flip flop.

Componente de memoria, timing planeamiento de tiempo, son pasivos, estado de excitación

Borde positivo y negativo de los ciclos de reloj.

Componente de memoria siempre tiene salida prima, siempre debe ser estable, si Q es 1, Q` debe ser 0, sino es inestable.

### Flip flop D

<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/d2b4a434-7ecd-4c03-b975-3b0692d6d0e0" />

En el momento que recibe un borde positivo, o negativa. Permite enviar el resultado almacenado según reciba señales del pulso del reloj en los bordes.

Recibe un flujo de datos llamado D, en el primer instante Q es 0, cuando recibe dicha señal de reloj, este cambia al valor asignado en el momento o estado del momento

Durante el tiempo que no recibe pulsos del reloj, el elemento de memoria al ser pasivo no reacciona hasta que no tenga otro cambio de reloj, al llegar al nuevo pulso de reloj, obtiene el nuevo valor de D que coincida en el momento.

### Flip Flop JK

Tiene control debido a que tiene 2 entradas, solo por eso tiene control.

<img width="400" height="248" alt="image" src="https://github.com/user-attachments/assets/12de649c-50de-4dba-bc76-b5c97b8d8c4d" />

<img width="310" height="248" alt="image" src="https://github.com/user-attachments/assets/5515d233-7e88-4358-aee3-28299b12047e" />

### Tabla de caracteristica

> Usar v en vez de t

**Flip Flop JK**

|JK|Q(v+1)
|:--:|:--:|
|00|Q(v) Sin Cambio
|01|0 Restablecer
|10|1 Establecer
|11|Q`(v) Complementar

**Flip Flop D**

|D|Q(v+1)
|:--:|:--:|
|0|0 Restablecer
|1|1 Establecer

**Flip Flop T**

|T|Q(v+1)
|:--:|:--:|
|0|Q(v) Sin cambio
|1|Q`(v) Complementar

#### Flip Flop JK

Según las tablas de caracteristica, el dibujo de cada flip flop en ASCII con el valor según se entregue VCC o GND en JK y la salida según CLK mande señales

* JK 00
* JK 01
* JK 10
* JK 11

Aplicaciones más comunes para flip flop puede ser un contador

## Ejemplo Contador con flip flop D

Contador que cuenta 0->1->2->3 y se reinicia, mayor 11)2, 2bit al usar 2 Flip Flop D ambos conectados al mismo reloj al ser contador sincronico.

> Siempre LSB es el conectado al reloj global al estar más cerca











# Unidad 1

## Bitácora de proceso de aprendizaje

### Actividad 01

¿Qué sucede?

El programa carga dos valores en los registros A y D, realiza una suma entre ellos y guarda un valor en la dirección de memoria 16, al final entra en un ciclo infinito

¿Qué valor se almacena en la dirección de memoria 16?_

El número 3

¿Por qué crees que es ese valor?

Por la linea de código "M=D" donde "M" es la memoria RAM y "D" uo de los registros en el que se almacenó el número 3 previamente

¿Qué instrucciones se ejecutan en cada ciclo Fetch-Decode-Execute?

- Se guarda el valor 1 en el registro A
- Hace que el registro D tenga el mismo valor del registro A
- Se sobreescribe en el registro A el valor 2
- Se suman los valores de los registros D y A, y el resultado se guarda en el registro D
- Le da un nuevo valor al registro A
- Almacena el valor que tiene el registro D, 3, en la direccion de memoria del valor del registro A (Address)
- Almacena el valor del numero del ROM que tenga la etiqueta "END"
- Genera un valor que, al final no se guarda y despues hace un salto a la memoria ROM que dice en el registro A

¿Qué cambios observas en el contenido de la memoria y los registros?

El registro A y D cambian durante la ejecución del programa, y como resultado final el valor 3 queda almacenado en la dirección de memoria 16, los registros PC y A quedan con el valor 7 y el registro queda con el valor de 3. El programa termina en un bucle infinito al ejecutar "0;JMP" hacia la etiqueta END

Experimento
``` 
@10
D=A
@5
D=D+A
@20
M=D
@END
(END)
0;JMP
```

<img width="1246" height="568" alt="image" src="https://github.com/user-attachments/assets/2ec0926a-d318-4a9f-a2a1-b8d59fcba866" />


### Actividad 02

Identifica una instrucción que use la ALU y explica qué hace

ALU (Arithmetic Logic Unit) es la parte de la CPU que suma, resta, niega, compara o hace operaciones logicas(AND, OR, etc.)

Una instrucción que usa la ALU es:

```
D=D-A
```
Esta instrucción utiliza la ALU para realizar una resta entre los valores almacenados en los registros D y A
El resultado de la operación se guarda nuevamente en el registro D

¿Para qué sirve el registro PC?

El registro PC (Program Counter) se encarga de almacenar la dirección de la siguiente instrucción que debe ejecutar la CPU, normalmente el PC avanza de forma secuencial, pero cuando el programa encuentra una instrucción de salto (JMP, JNE, JLE, JGE), el PC cambia su valor y el programa continúa desde otra parte del código

¿Cuál es la diferencia entre @i y @READKEYBOARD?

- @i hace referencia a una variable almacenada en la memoria RAM, que en este programa se utiliza como un puntero para recorrer la memoria de la pantalla
- @READKEYBOARD hace referencia a una etiqueta del programa, es decir, a una posición del código a la cual se puede saltar para repetir el proceso de lectura del teclado

Describe qué se necesita para leer el teclado y mostrar información en la pantalla

Para leer el teclado, el programa accede a la dirección de memoria @KBD y obtiene su valor, el cual indica si una tecla está presionada o no, para mostrar información en la pantalla, el programa escribe valores en la memoria que comienza en @SCREEN, además, se necesita un bucle infinito que permita revisar constantemente el estado del teclado y condiciones que determinen si se debe escribir o borrar información en la pantalla

Identifica un bucle en el programa y explica su funcionamiento

```
@READKEYBOARD
0;JMP
```
Este salto incondicional hace que el programa vuelva constantemente a la etiqueta READKEYBOARD, esta condición se utiliza para detectar si una tecla está presionada y decidir si se debe dibujar información en la pantalla


### Actividad 3

Escribe un programa que compare el valor almacenado en la dirección de memoria 5 con el valor 10. Si el valor es menor que 10, guarda el valor 1 en la dirección 7. Si el valor es mayor o igual a 10, guarda el valor 0 en la dirección 7.

```
@5
D=M
@10
D=D-A
@MENOR
D;JLT

@7
M=0
@FINAL
0;JMP

(MENOR)
@7
M=1

@FINAL
(FINAL)
0;JMP

```

##Actividad 04

Crea un programa que use un ciclo para sumar los números del 1 al 5 y guarde el resultado en la dirección de memoria 12

```
@i
M=1

(LOOP)
@i
D=M
@6
D=A-D
@FINAL
D;JEQ

@i
D=M

@sum
M=D+M

@i
M=M+1
@LOOP
0;JMP

(FINAL)
@sum
D=M
@12
M=D
@END
(END)
0;JMP
```
## Bitácora de aplicación 



## Bitácora de reflexión



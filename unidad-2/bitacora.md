# Unidad 2

## Bitácora de proceso de aprendizaje
### Actividad 01
```
(START)
@SCREEN
M=1

@FIN
(FIN)
0;JMP
```

### Actividad 02
```
(START)
@SCREEN
M=-1

@FIN
(FIN)
0;JMP
```
Actividad 03
```
(START)
//i = SCREEN
@SCREEN
D=A
@i
M=D
// Inicio la pantalla pintando una linea
@SCREEN
M=-1

// Detecta si se esta presionando "d" o "i"
(LOOP)
@KBD
D=M
@100
D=D-A
@DERECHA
D;JEQ

@KBD
D=M
@105
D=D-A
@IZQUIERDA
D;JEQ
@LOOP
0;JMP

// Si se presiona "d" se moverá a la derecha
(DERECHA)
@i
A=M
M=0
A=A+1
M=-1
D=A
@i
M=D

@LOOP
0;JMP

// Si se presiona "i" se moverá a la izquierda
(IZQUIERDA)
@i
A=M
M=0
A=A-1
M=-1
D=A
@i
M=D

@LOOP
0;JMP

@FIN
(FIN)
0;JMP
```
### Actividad 05
```
@10
D=A
@a
M=D

@5
D=A
@b
M=D

@a
D=A
@p
M=D

@20
D=A
@p
A=M
M=D
```

```
@10
D=A
@a
M=D

@5
D=A
@b
M=D

@a
D=A
@p
M=D

@p
A=M
D=M
@b
M=D
```

## Bitácora de aplicación 

### Actividad 08
```
@10
D=A
@16
M=D    // RAM[16] = a = 10

@20
D=A
@17
M=D    // RAM[17] = b = 20

@16
D=A
@R0
M=D    

@17
D=A
@R1
M=D

// SE GUARDAN LAS &a y &b EN R0 Y R1 

@retornoDespuesCambio
D=A
@R15
M=D

@swap
0;JMP

// DIRECCIÓN RETORNO

(retornoDespuesCambio)
@END
0;JMP

(END)
0;JMP

(swap)

@R0
A=M
D=M
@13
M=D

@R1
A=M
D=M
@R0
A=M
M=D

@13
D=M
@R1
A=M
M=D

@15
A=M
0;JMP
```
<img width="589" height="775" alt="image" src="https://github.com/user-attachments/assets/e25c91fd-03c9-4092-9c43-4219a232ceda" /> <img width="1002" height="750" alt="image" src="https://github.com/user-attachments/assets/6de32cf5-7017-4ccf-beab-46f7929978ca" />

<img width="1232" height="574" alt="image" src="https://github.com/user-attachments/assets/b30e5069-2ba8-4a7d-aeeb-0b12e09f6218" />
<img width="1225" height="753" alt="image" src="https://github.com/user-attachments/assets/7210fa6a-c95e-4909-ba79-491906ba9126" />
<img width="1231" height="740" alt="image" src="https://github.com/user-attachments/assets/d2e18171-f080-493c-8203-b552768dc351" />
<img width="1230" height="784" alt="image" src="https://github.com/user-attachments/assets/9443f0e4-731c-45df-b106-e3773cc73536" />


## Bitácora de reflexión





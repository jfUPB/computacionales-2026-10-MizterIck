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

### Actividad 09

## Bitácora de reflexión



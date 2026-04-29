# Unidad 7

## Bitácora de proceso de aprendizaje
### Actividad 02

Primero está GLFW, que es la biblioteca que se encarga de crear la ventana y manejar cosas como el teclado y el mouse. Además, GLFW también crea el contexto de OpenGL, que es como el espacio donde OpenGL puede funcionar. Sin esto, no podríamos dibujar nada.

Luego está opengl32.lib, que es una biblioteca que ya viene con Windows. Esta permite enlazar nuestro programa con OpenGL, pero solo con funciones básicas (hasta la versión 1.1). Es como una puerta de entrada a OpenGL, pero bastante limitada.

Ahí es donde entra GLAD. Como las funciones modernas de OpenGL no están en opengl32.lib, necesitamos una forma de acceder a ellas. GLAD se encarga de cargar esas funciones directamente desde los drivers de la GPU en tiempo de ejecución. Es decir, GLAD actúa como un intermediario que conecta nuestro programa con las capacidades reales de la tarjeta gráfica.

Los drivers de la GPU son muy importantes porque ahí es donde realmente están implementadas las funciones modernas de OpenGL. Cuando usamos OpenGL, en realidad estamos enviando instrucciones a los drivers, y estos hacen que la GPU ejecute el trabajo de dibujar.

Finalmente está GLM, que es una biblioteca de matemáticas. No es obligatoria, pero es muy útil porque nos permite trabajar con vectores, matrices y transformaciones, que son fundamentales en gráficos 3D.

### Actividad 03

(1)

Al modificar los valores de glViewport, observé que el triángulo no cambia su forma, pero sí su tamaño y posición en la pantalla.

Cuando dividí el width y height, el triángulo se hizo más pequeño y apareció en la esquina inferior izquierda. Esto ocurre porque el área donde se dibuja (el viewport) es más pequeña.

Cuando multipliqué estos valores, el triángulo se hizo más grande y parte de él salió de la pantalla, ya que el viewport supera el tamaño del framebuffer.

(2)

Hasta ahora he aprendido cómo funciona la estructura básica de un programa en OpenGL. Para poder dibujar, primero se necesita crear una ventana y un contexto OpenGL usando GLFW. Este contexto es importante porque es donde OpenGL guarda todo su estado y donde se ejecutan las operaciones gráficas.

También que OpenGL no dibuja directamente, sino que actúa como intermediario entre el programa y la GPU, que es quien realmente realiza el dibujo en el framebuffer. El framebuffer es una región de memoria donde se almacenan los píxeles antes de mostrarse en pantalla.

El viewport define qué parte del framebuffer se usa para dibujar, y que cambiar sus valores no modifica la geometría del objeto, sino su tamaño y posición en pantalla.

Además, la estructura del ciclo principal (game loop), donde en cada iteración se procesan eventos, se limpia la pantalla, se dibuja la escena y se actualiza lo que se muestra en pantalla.

En general, el flujo consiste en enviar instrucciones desde el CPU usando OpenGL, que luego son ejecutadas por la GPU para generar la imagen final.

(3)

Cuando cambié a GL_LINES, el triángulo dejó de verse como tal y en su lugar apareció una línea. Esto ocurre porque OpenGL ahora interpreta los vértices en pares, dibujando segmentos de línea entre ellos en lugar de formar triángulos.

Al usar GL_POINTS, solo se mostraron puntos en la pantalla. En mi caso, observé un punto visible hacia el lado derecho, lo que indica que OpenGL dibuja cada vértice como un punto individual, sin conectarlos entre sí.

Por otro lado, al modificar el tercer parámetro de glDrawArrays (la cantidad de vértices), noté que cuando el valor era menor a 3, el triángulo desaparecía. Esto se debe a que un triángulo necesita al menos tres vértices para poder formarse.

Cuando el valor era mayor a 3, el resultado visual no cambiaba en este caso, ya que solo se habían definido tres vértices en el buffer, por lo que OpenGL no tenía más datos válidos para dibujar.

(4)

¿Qué es el contexto OpenGL?

Es como el “espacio de trabajo” donde OpenGL guarda todo lo que necesita para funcionar: shaders, buffers, configuraciones, etc.
Si no hay contexto, OpenGL básicamente no puede hacer nada, porque no sabe dónde dibujar ni qué recursos tiene disponibles.

¿Cuál es el rol de GLFW y qué ventaja tiene usarla?

GLFW es la que se encarga de crear la ventana y el contexto OpenGL, además de manejar teclado, mouse, etc.
La ventaja es que te ahorra lidiar con cosas específicas del sistema operativo. O sea, no tienes que programar diferente para Windows, Linux o Mac.

¿Por qué OpenGL necesita un contexto?

Usando la analogía: OpenGL es el artista, pero necesita un taller (el contexto).
Sin ese taller, no tiene dónde guardar sus herramientas ni dónde trabajar. El contexto le dice qué recursos existen y dónde se va a dibujar.

¿Qué es el framebuffer y a qué te recuerda?

El framebuffer es como una “pantalla interna” donde la GPU dibuja todo antes de mostrarlo.
Me recuerda a cuando en unidades anteriores trabajábamos con una matriz de píxeles o una imagen en memoria antes de mostrarla.

¿Relación entre viewport y framebuffer?

El framebuffer es toda la “hoja” donde se dibuja.
El viewport es una parte de esa hoja.

Es como decir:

framebuffer = toda la pantalla
viewport = el área donde decides dibujar

¿Qué rol juegan los drivers y la GPU?

La GPU es la que hace el trabajo pesado: dibujar, procesar vértices, etc.
Los drivers son como el traductor entre OpenGL y la GPU.

O sea:

tú = escribes código OpenGL
OpenGL = manda instrucciones
drivers = las traducen
GPU = ejecuta todo

¿Por qué activar VSync?

VSync sincroniza los frames con la pantalla para evitar que se “rompa” la imagen (tearing).

Si la imagen es estática → casi no pasa nada si lo quitas
Si es dinámica → se puede ver como “cortada” o desalineada

¿Qué es OpenGL Legacy vs moderno?

Legacy: tenía funciones fijas (tipo glBegin, glEnd), menos control
Moderno: usa shaders, tú decides cómo funciona el pipeline

La diferencia clave:
antes OpenGL decidía cómo dibujar
ahora tú programas cómo dibujar

¿Qué es el shader program?

Es el programa que corre en la GPU y le dice cómo procesar los datos:

vertex shader → qué hacer con los vértices
fragment shader → qué color poner

Es obligatorio en OpenGL moderno, sin shaders no se dibuja nada.

¿Qué hace setupTriangle()? ¿Qué son VAO y VBO?

Intuitivamente:

define los vértices del triángulo
los manda a la GPU

VBO:
guarda los datos (los vértices)

VAO:
guarda cómo usar esos datos (configuración)


## Bitácora de aplicación 


## Bitácora de reflexión

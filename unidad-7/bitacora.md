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

### Actividad 4

(1)

La CPU es como el “cerebro general” del computador. Está pensada para ejecutar pocas tareas a la vez, pero de forma muy rápida y con mucha lógica. Es la que maneja el programa, toma decisiones, ejecuta instrucciones complejas y coordina todo.

La GPU, en cambio, está diseñada para hacer muchísimas operaciones simples al mismo tiempo. No es tan buena tomando decisiones complejas, pero es excelente haciendo cálculos repetitivos en paralelo, como los que se necesitan en gráficos.

(2)

¿Cuáles son los tres pasos claves del pipeline de OpenGL?

1. Procesamiento de vértices (Vertex Processing)
Aquí la GPU toma los vértices que le manda la CPU y los transforma (posición, escala, etc.). Esto lo hace el vertex shader.
Básicamente: “¿Dónde va cada punto en la pantalla?”

2. Rasterización
Aquí se convierten las figuras (como triángulos) en fragmentos, que son como candidatos a píxeles.
Básicamente: “¿Qué partes de la pantalla cubre ese triángulo?”

3. Procesamiento de fragmentos (Fragment Processing)
Se define el color final de cada fragmento usando el fragment shader.
Básicamente: “¿De qué color es cada pedacito?”

¿Qué es el pipeline programable?

Antes (pipeline fijo): 
OpenGL ya tenía todo definido: tú solo enviabas datos.

Ahora (pipeline programable):
tú decides cómo se procesan los vértices y los fragmentos usando shaders.

Diferencia:

Pipeline fijo → menos control
Pipeline programable → tú programas el comportamiento

Ventajas:

Más control visual
Puedes hacer efectos personalizados
Más flexibilidad

¿Qué programas?

Vertex shader
Fragment shader

¿Cómo describirías la rasterización?

Es el proceso donde OpenGL toma una figura (como un triángulo) y la convierte en muchos fragmentos que cubren su área. Es como “rellenar” el triángulo con pequeños puntos que luego se convertirán en píxeles.

¿Qué son los fragmentos? ¿Son lo mismo que píxeles?

No exactamente.

Fragmento → es un candidato a píxel
Píxel → es el resultado final en pantalla

Un fragmento puede ser descartado (por profundidad, transparencia, etc.), así que no todos terminan siendo píxeles.

¿Qué problema resuelve el Z-buffer? ¿Qué es el depth test?

El problema es:
¿qué objeto se ve delante y cuál detrás?

El Z-buffer guarda la profundidad de cada fragmento.

El depth test compara:

si un fragmento está más cerca = se dibuja
si está más lejos = se descarta

Esto evita que objetos lejanos tapen a los cercano

¿Por qué ocurre el aliasing? ¿Qué es el anti-aliasing?

El aliasing pasa porque estamos representando líneas o bordes en una cuadrícula de píxeles.

Resultado: bordes “dentados” o pixelados.

El anti-aliasing intenta suavizar esos bordes, haciendo que se vean más naturales.

Relación entre iluminación y fragment shader

El fragment shader es donde normalmente se calcula el color final, incluyendo iluminación.

Puedes hacer:

shaders con iluminación (más realistas)
shaders sin iluminación (color plano)

Implicación:

Con iluminación equivale a más realismo, más costo computacional
Sin iluminación equivale a algo más simple, más rápido

¿Qué implica tener múltiples fuentes de iluminación?

Para la GPU significa más cálculos.

Cada luz implica:

más operaciones matemáticas
más trabajo en el fragment shader

Resultado:

mejor calidad visual
pero menor rendimiento si hay muchas luces

(3)

¿Qué se necesita para dibujar un triángulo en OpenGL?

Para dibujar un triángulo en OpenGL no basta con decir “dibuja un triángulo”, hay que preparar varias cosas.

Primero, se necesita crear una ventana y un contexto OpenGL (usando GLFW), porque sin eso no hay dónde dibujar. Luego, hay que cargar las funciones de OpenGL con GLAD para poder usar la API moderna.

Después, se deben definir los vértices del triángulo en el código (las posiciones de sus tres puntos). Esos datos se envían a la GPU usando un VBO, y se configura un VAO para indicarle a OpenGL cómo interpretar esos datos.

También es necesario crear y usar un shader program, porque en OpenGL moderno los shaders son obligatorios. El vertex shader se encarga de posicionar los vértices y el fragment shader define el color del triángulo.

Finalmente, dentro del ciclo principal, se limpia la pantalla, se activa el shader, se activa el VAO y se llama a glDrawArrays para dibujar el triángulo, y luego se intercambian los buffers para mostrar el resultado.

¿Qué se necesita para poder usar un shader en OpenGL?

Para usar un shader en OpenGL, primero hay que escribir el código del shader (vertex y fragment shader) en GLSL.

Luego, en C++, se crea cada shader, se compila y se verifica que no tenga errores. Después, se crea un shader program donde se enlazan (linkean) ambos shaders.

Una vez creado el programa, se puede activar con glUseProgram, y a partir de ese momento OpenGL usará ese shader para procesar los datos.

Además, si el shader necesita datos externos (como colores o posiciones dinámicas), se pueden enviar desde C++ usando uniforms.

(4)

Estoy dibujando tres triángulos usando el mismo conjunto de datos, pero cada uno utiliza un shader diferente. Cada shader interpreta los atributos de forma distinta (posición, color u offset), por lo que los triángulos cambian de tamaño o ubicación. Aunque todos tienen el mismo color porque el fragment shader es igual, la geometría cambia dependiendo del atributo usado.

<img width="398" height="392" alt="image" src="https://github.com/user-attachments/assets/2d734df3-6aed-4722-991c-55e5040e894e" />

### Actividad 5

<img width="791" height="699" alt="image" src="https://github.com/user-attachments/assets/59dbd603-ec90-4754-b40d-9e3063412c26" />

Normalización de las coordenadas del mouse

Cuando obtenemos la posición del mouse con glfwGetCursorPos, los valores (xpos, ypos) están en coordenadas de pantalla, es decir:

Van desde 0 hasta el ancho de la ventana en X
Y desde 0 hasta el alto de la ventana en Y
El origen (0,0) está en la esquina superior izquierda

Pero OpenGL no trabaja con ese sistema. Por eso necesitamos normalizar esos valores.

Lo que hacemos es dividir:

x = xpos / SCR_WIDTH
y = ypos / SCR_HEIGHT

Con esto convertimos las coordenadas a un rango de 0 a 1, donde:

0 representa el inicio de la pantalla
1 representa el final

Además, se hace un pequeño ajuste para asegurarnos de que los valores no se salgan de ese rango (clamping).

Normalización a coordenadas de dispositivo (NDC)

OpenGL trabaja internamente con un sistema llamado NDC (Normalized Device Coordinates).

En este sistema:

X y Y van de -1 a 1
(0,0) está en el centro de la pantalla
(-1,-1) es la esquina inferior izquierda
(1,1) es la esquina superior derecha

Entonces, después de tener valores entre 0 y 1, hacemos otra transformación:

x_ndc = x * 2 - 1
y_ndc = 1 - y * 2

Esto hace dos cosas:

Convierte el rango de [0,1] → [-1,1]
Invierte el eje Y (porque en pantalla Y crece hacia abajo, pero en OpenGL hacia arriba)


## Bitácora de aplicación 

### Actividad 6

Fase 1



## Bitácora de reflexión

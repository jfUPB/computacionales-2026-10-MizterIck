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



## Bitácora de aplicación 


## Bitácora de reflexión

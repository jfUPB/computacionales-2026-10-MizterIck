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

``` c++
#include <iostream>
#include <glad/glad.h>
#include <GLFW/glfw3.h>

// Callback viewport
void framebuffer_size_callback(GLFWwindow* window, int width, int height) {
    glViewport(0, 0, width, height);
}

// Input
void processInput(GLFWwindow* window) {
    if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
}

const unsigned int SCR_WIDTH = 600;
const unsigned int SCR_HEIGHT = 600;

// Vertex Shader
const char* vertexShaderSrc = R"glsl(
#version 460 core
layout(location = 0) in vec3 aPos;

uniform vec2 offset;

void main() {
    vec3 newPos = aPos;
    newPos.x += offset.x;
    newPos.y += offset.y;
    gl_Position = vec4(newPos, 1.0);
}
)glsl";

// Fragment Shader
const char* fragmentShaderSrc = R"glsl(
#version 460 core
out vec4 FragColor;

uniform vec4 ourColor;

void main() {
    FragColor = ourColor;
}
)glsl";

unsigned int VAO, VBO;
unsigned int shaderProg;

// Crear shader program
unsigned int buildShaderProgram(const char* vertexSrc) {
    int success;
    char log[512];

    unsigned int vs = glCreateShader(GL_VERTEX_SHADER);
    glShaderSource(vs, 1, &vertexSrc, nullptr);
    glCompileShader(vs);
    glGetShaderiv(vs, GL_COMPILE_STATUS, &success);
    if (!success) {
        glGetShaderInfoLog(vs, 512, nullptr, log);
        std::cerr << "ERROR VERTEX:\n" << log << "\n";
    }

    unsigned int fs = glCreateShader(GL_FRAGMENT_SHADER);
    glShaderSource(fs, 1, &fragmentShaderSrc, nullptr);
    glCompileShader(fs);
    glGetShaderiv(fs, GL_COMPILE_STATUS, &success);
    if (!success) {
        glGetShaderInfoLog(fs, 512, nullptr, log);
        std::cerr << "ERROR FRAGMENT:\n" << log << "\n";
    }

    unsigned int prog = glCreateProgram();
    glAttachShader(prog, vs);
    glAttachShader(prog, fs);
    glLinkProgram(prog);
    glGetProgramiv(prog, GL_LINK_STATUS, &success);
    if (!success) {
        glGetProgramInfoLog(prog, 512, nullptr, log);
        std::cerr << "ERROR LINK:\n" << log << "\n";
    }

    glDeleteShader(vs);
    glDeleteShader(fs);

    return prog;
}

// Triángulo
void setupTriangle() {
    float vertices[] = {
        -0.2f, -0.2f, 0.0f,
         0.2f, -0.2f, 0.0f,
         0.0f,  0.2f, 0.0f
    };

    glGenVertexArrays(1, &VAO);
    glGenBuffers(1, &VBO);

    glBindVertexArray(VAO);
    glBindBuffer(GL_ARRAY_BUFFER, VBO);
    glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

    glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
    glEnableVertexAttribArray(0);

    glBindVertexArray(0);
}

int main() {

    glfwInit();
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 6);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

    GLFWwindow* mainWindow = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "Triangulo Interactivo", NULL, NULL);
    if (!mainWindow) {
        std::cout << "Error creando ventana\n";
        glfwTerminate();
        return -1;
    }

    glfwMakeContextCurrent(mainWindow);
    glfwSetFramebufferSizeCallback(mainWindow, framebuffer_size_callback);

    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)) {
        std::cout << "Error GLAD\n";
        return -1;
    }

    glfwSwapInterval(1);

    shaderProg = buildShaderProgram(vertexShaderSrc);
    setupTriangle();

    glUseProgram(shaderProg);
    int offsetLocation = glGetUniformLocation(shaderProg, "offset");
    int colorLocation = glGetUniformLocation(shaderProg, "ourColor");

    while (!glfwWindowShouldClose(mainWindow))
    {
        glfwPollEvents();
        processInput(mainWindow);

        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        glUseProgram(shaderProg);

        double xpos, ypos;
        glfwGetCursorPos(mainWindow, &xpos, &ypos);

        float x = (float)xpos / (float)SCR_WIDTH;
        if (x < 0) x = 0;
        if (x > 1) x = 1;

        float y = (float)ypos / (float)SCR_HEIGHT;
        if (y < 0) y = 0;
        if (y > 1) y = 1;

        glUniform4f(colorLocation, x, y, 0.0f, 1.0f);
        glUniform2f(offsetLocation, x * 2 - 1, 1 - y * 2);

        glBindVertexArray(VAO);
        glDrawArrays(GL_TRIANGLES, 0, 3);

        glfwSwapBuffers(mainWindow);
    }

    glDeleteVertexArrays(1, &VAO);
    glDeleteBuffers(1, &VBO);
    glDeleteProgram(shaderProg);

    glfwDestroyWindow(mainWindow);
    glfwTerminate();
    return 0;
}

```

Fase 2

Evidencia 1
<img width="932" height="454" alt="image" src="https://github.com/user-attachments/assets/f3a158c0-fb7d-44ff-a6d7-5f49735afff3" />

Explicación de la evidencia

En la captura se observa que el programa ha ejecutado glfwMakeContextCurrent(mainWindow) antes de llamar a gladLoadGLLoader. Esto indica que el contexto de OpenGL ya fue creado y activado por GLFW.

Además, se puede ver que tanto mainWindow como glfwGetProcAddress tienen direcciones válidas en memoria, lo que confirma que GLFW inicializó correctamente el entorno antes de que GLAD intente cargar las funciones de OpenGL.

Justificación

GLFW debe ejecutarse antes que GLAD porque es el encargado de crear el contexto de OpenGL. GLAD, por su parte, necesita ese contexto activo para poder obtener las direcciones de las funciones de OpenGL desde los drivers de la GPU.

Si se intentara cargar GLAD antes de crear el contexto, la carga fallaría porque no existiría un entorno válido desde el cual obtener dichas funciones.

Evidencia 2
<img width="938" height="518" alt="image" src="https://github.com/user-attachments/assets/75a5db22-3929-4717-ac51-8d3fb222f4b9" />
<img width="929" height="621" alt="image" src="https://github.com/user-attachments/assets/d7e83c10-d678-44dd-b65b-f99c28e21b12" />

Explicación de la evidencia

En la primera captura se observa el arreglo vertices definido en el código C++, el cual contiene las coordenadas de los vértices del triángulo. En ese momento, los datos aún se encuentran en la memoria de la CPU y están siendo enviados a la GPU mediante la función glBufferData, lo que implica su almacenamiento en un Vertex Buffer Object (VBO).

En la segunda captura se muestra el momento previo a la ejecución de glDrawArrays, donde el programa de shaders ha sido activado con glUseProgram(shaderProg) y el VAO se encuentra enlazado mediante glBindVertexArray(VAO). Además, se están enviando valores a los uniforms del shader, lo que indica que el pipeline ya está completamente configurado.

Esto evidencia el flujo completo de datos: desde su definición en CPU, pasando por su transferencia a la GPU, su configuración mediante el VAO, hasta su uso final en el shader para generar la imagen en pantalla.

Justificación

Estas capturas demuestran que el acceso a los datos por parte del shader no es directo, sino que ocurre a través del pipeline de OpenGL. Primero, los datos son transferidos desde la CPU a la GPU mediante un VBO. Luego, el VAO define cómo deben interpretarse esos datos y a qué atributos del shader están asociados. Finalmente, cuando se ejecuta glDrawArrays, OpenGL toma esos datos ya configurados y los envía al vertex shader activo.

Esto confirma que existe una separación clara entre la gestión de datos en la CPU y su procesamiento en la GPU, y que el flujo de información depende del estado configurado en OpenGL (VAO, VBO y shader program), no de accesos directos a memoria desde el shader.

Evidencia 3

<img width="927" height="419" alt="image" src="https://github.com/user-attachments/assets/aa712803-139e-476a-8482-293446b271fe" />
<img width="939" height="413" alt="image" src="https://github.com/user-attachments/assets/bd1e7be9-1fb0-44ea-9c10-c8770708c16a" />
<img width="932" height="408" alt="image" src="https://github.com/user-attachments/assets/e094841f-068b-477c-bced-5c01d4f28a21" />

Explicación de la evidencia

En la primera captura se observa cómo el arreglo de vértices es enviado al GPU mediante glBufferData, estableciendo el contenido del VBO. Este proceso ocurre una sola vez durante la inicialización y no se vuelve a ejecutar dentro del loop principal.

En las capturas posteriores se observa cómo los valores de los uniforms (x, y) cambian dinámicamente en cada frame, afectando la posición y el color del triángulo en pantalla.

Esto demuestra que el VBO permanece constante y que los cambios visuales se logran mediante uniforms, los cuales modifican el comportamiento del shader sin alterar los datos originales.

Justificación

El comportamiento observado se explica por la forma en que funciona el pipeline de OpenGL y la separación de responsabilidades entre sus componentes.

El Vertex Buffer Object (VBO) almacena los datos de los vértices en la memoria de la GPU y, en este caso, se inicializa una sola vez mediante la función glBufferData. Como este proceso ocurre fuera del loop principal, los datos permanecen constantes durante toda la ejecución del programa.

Por otro lado, los uniforms (offset y ourColor) son variables globales dentro del shader que pueden actualizarse en cada frame desde el código C++. Estas variables no modifican los datos del VBO, sino que alteran la forma en que el shader interpreta esos datos:

El offset modifica la posición de los vértices en el vertex shader.
El ourColor define el color final en el fragment shader.

Esto implica que el mismo conjunto de vértices puede producir diferentes resultados visuales sin necesidad de modificar la geometría original.

En términos del pipeline, el VBO representa los datos de entrada, mientras que los uniforms representan parámetros de procesamiento dinámico. Esta separación permite mayor eficiencia, ya que evita tener que reenviar datos a la GPU en cada frame y delega las transformaciones al shader.

Evidencia 4

<img width="1570" height="906" alt="image" src="https://github.com/user-attachments/assets/4c717f7c-a640-4003-8d99-3e89f1245a7e" />

Cambiar el offset que se tenía en el código por

``` c++
glUniform2f(offsetLocation, 2.0f, 2.0f);
```

¿Qué esperaba que pasara?

Se esperaba que en este experimento el triangulo desapareciera de la pantalla, puesto a que este se desplazaría fuera de esta.

¿Que ocurrió?

Efectivamente el triangulo desapareció de la pantalla, sin errores de consola ni nada extraño.

Conclusiones

El resultado muestra que el triángulo desaparece porque el offset lo mueve fuera del rango visible de las coordenadas NDC ([-1, 1]). En esta etapa del pipeline, OpenGL aplica clipping, descartando automáticamente cualquier geometría fuera de ese espacio.

Es importante notar que el VBO no cambia; los datos del triángulo siguen iguales en memoria. El cambio visual ocurre únicamente por el uniform offset, que modifica la posición de los vértices en el vertex shader.

En conclusión, esto demuestra que los uniforms afectan el resultado visual sin modificar la geometría, y que muchos errores visuales provienen del estado del pipeline y no de los datos originales.

Evidencia 5

<img width="929" height="420" alt="image" src="https://github.com/user-attachments/assets/3c1b5680-e341-47a2-bca1-82ae8a2893bb" />

Usar offset como uniform en lugar de atributo de vértice

Explicación

Decidí implementar el desplazamiento del triángulo usando un uniform (offset) en lugar de un atributo porque el movimiento que quiero aplicar es el mismo para todos los vértices.

Un atributo de vértice sirve para enviar datos diferentes por vértice (como posición o color), mientras que un uniform es un valor global que se mantiene constante durante todo el draw call. En este caso, el desplazamiento es único para toda la figura, por lo que tiene más sentido usar un uniform.

Justificación

Usar un uniform es más eficiente y adecuado porque evita modificar el VBO cada frame. Si hubiera usado un atributo, tendría que actualizar los datos de cada vértice constantemente, lo cual implica más operaciones en CPU y transferencia de datos a la GPU.

Además, al usar un uniform, el cálculo del desplazamiento se realiza directamente en el vertex shader, aprovechando la GPU y manteniendo el VBO como un conjunto de datos estáticos.

Esto respeta la lógica del pipeline de OpenGL, donde:

El VBO almacena la geometría base
Los uniforms controlan transformaciones globales
El shader aplica las modificaciones en tiempo real


## Bitácora de reflexión

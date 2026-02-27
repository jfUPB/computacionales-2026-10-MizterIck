# Unidad 3

## Bitácora de proceso de aprendizaje
### Actividad 02
``` c++
#include <iostream>

using namespace std;

void swapPorValor(int a, int b)
{
    int c = b;

    b = a;
    
    a = c;
}

void swapPorReferencia(int& a, int& b) 
{
    int c = b;
    
    b = a;

    a = c;
}

void swapPorPuntero(int* a, int* b) 
{
    int c = *b;

    *b = *a;

    *a = c;

}

int main() {
    int a = 10;
    int b = 20;

    swapPorPuntero(&a, &b);

    cout << "a: " << a << endl;
    cout << "b: " << b << endl;

    return 0;
}
```

### Actividad 04

- Experimento 1: modificar el segmento de texto
	
``` c++
#include <iostream>
#include <cstdlib>

using namespace std;


int main() {
    // Variable local (stack)
    int a = 10;
    int b = 20;

    /**********************************************************
        EXPERIMENTO 1
    ***********************************************************/

    void* ptr = reinterpret_cast<void*>(&main);
    cout << "Voy a modificar la memoria en la dirección: " << ptr << endl;
    *reinterpret_cast<int*>(ptr) = 0;

    /********************************************************/

    return 0;
}
```

- Al ejecutar el programa, este normalmente se detiene con un error (segmentation fault o access violation). Es decir, el programa se cae antes de terminar correctamente.
- Porque el código intenta modificar la memoria donde está almacenada la función main. Esa zona pertenece al segmento de código (Text), que en los sistemas modernos está protegida como solo lectura y ejecutable, pero no escribible.
- Cuando el programa ejecuta esta línea:

- `*reinterpret_cast<int*>(ptr) = 0;`

- está intentando escribir un valor entero directamente sobre las instrucciones compiladas de main. El sistema operativo detecta que se está intentando escribir en una región protegida de memoria y genera una excepción, finalizando el programa.

El segmento de texto no puede modificarse porque está protegido por el sistema operativo para evitar errores y posibles ataques. Este experimento demuestra que el código ejecutable vive en una región de memoria distinta y protegida frente a escritura.


- Experimento 2: modificar el segmento de datos (constante global)
	
``` c++
#include <iostream>
#include <cstdlib>

using namespace std;

// Constante global
const char* const mensaje_ro = "Hola, memoria de solo lectura";


int main() {
    // Variable local (stack)
    int a = 10;
    int b = 20;


    /**********************************************************
        EXPERIMENTO 2
    ***********************************************************/

    char* ptr = (char*)&mensaje_ro;
    cout << "Voy a modificar la memoria en la dirección: " << ptr << endl;
    *ptr = 0;

    /********************************************************/

    return 0;
}
```
- el programa se cae cuando se intenta realizar la linea `*ptr = 0;`, generando una excepcion por infraccion de acceso de escritura; porque `ptr` apunta a la dirección 0x7FF76587AC10, que es &mensaje_ro —memoria de solo lectura— y la escritura en ella falla.
-  mensaje_ro está declarado como const char* const mensaje_ro = "Hola...". Eso significa que tanto el puntero como los datos apuntados son constantes; el compilador ubica ese objeto en una sección de solo lectura. En el código se hace (char*)&mensaje_ro y se asigna a char* ptr, luego se escribe *ptr = 0. Eso intenta modificar la memoria donde se almacena la variable mensaje_ro (dirección 0x7FF76587AC10 según el depurador), no el contenido de la cadena, y como esa región es de solo lectura el sistema lanza la excepción.

El programa falla porque intenta modificar una constante global, y esa zona de memoria está protegida como solo lectura para evitar cambios accidentales.

- Experimento 3: modificar el segmento de datos (variables globales)

``` c++
#include <iostream>
#include <cstdlib>

using namespace std;

// Variables globales
int global_inicializada = 42;
int global_no_inicializada;


int main() {
    // Variable local (stack)
    int a = 10;
    int b = 20;

    /**********************************************************
        EXPERIMENTO 3
    ***********************************************************/

    cout << "global_inicializada: " << global_inicializada << endl;
    cout << "global_no_inicializada: " << global_no_inicializada << endl;


    global_inicializada = 69;
    global_no_inicializada = 666;

    cout << "global_inicializada: " << global_inicializada << endl;
    cout << "global_no_inicializada: " << global_no_inicializada << endl;

    /********************************************************/

    return 0;
}
```
- El programa se ejecuta normalmente, primero imprime: `global_inicializada: 42` y `global_no_inicializada: 0`. Luego, después de modificarlas, imprime: `global_inicializada: 69` y `global_no_inicializada: 666`, y aca no ocurre ningun error
- Esto ocurre porque las variables globales sí pueden modificarse durante la ejecución del programa.
	- `global_inicializada` empieza en 42 porque fue declarada con un valor inicial.
	- `global_no_inicializada` aparece como 0 porque las variables globales que no se inicializan explícitamente se inicializan automáticamente en 0.

   Ambas están en la zona de datos globales, que es una región de memoria escribible, por eso se pueden cambiar sin problema.

A diferencia del segmento de código o de las constantes de solo lectura, las variables globales sí pueden modificarse durante la ejecución porque están almacenadas en una zona de memoria que permite escritura.

- Experimento 4: modificar la variable local estática de una función por fuera de ella

``` c++
#include <iostream>
#include <cstdlib>

using namespace std;

// Función de ejemplo que muestra la dirección de su variable local estática
void funcionConStatic() {
    static int var_estatica = 100;
    cout << "Dirección de var_estatica (static): " << &var_estatica << endl;
}


int main() {
    // Variable local (stack)
    int a = 10;
    int b = 20;

    /**********************************************************
        EXPERIMENTO 4
    ***********************************************************/

    var_estatica = 42;

    cout << "var_estatica: " << var_estatica << endl;

    /********************************************************/
    return 0;
}
```

- El programa directamente no compila. El compilador muestra un error diciendo que var_estatica no está declarada en este ámbito.
- Porque `var_estatica` fue declarada dentro de la función `funcionConStatic()`, aunque sea `static` (lo que significa que vive durante toda la ejecución del programa), su alcance sigue siendo local a esa función.

¿Qué pasa con las variables cada que entras y sales de la función?
- Las variables locales normales (no estáticas) se crean cada vez que entras a la función y se destruyen cuando sales de ella

¿Qué pasa con las variables locales estáticas?
- Las variables locales estáticas se crean una sola vez y conservan su valor entre llamadas a la función

- Experimento 5: variables locales estática vs no estática

``` c++
#include <iostream>
#include <cstdlib>

using namespace std;

// Función de ejemplo que muestra la dirección de su variable local estática
void funcionConStatic() {
    static int var_estatica = 100;
    cout << "var_estatica: " << var_estatica << endl;
    var_estatica++;
}

void funcionSinStatic() {
    int var_no_estatica = 100;
    cout << "var_no_estatica: " << var_no_estatica << endl;
    var_no_estatica++;
}


int main() {
    // Variable local (stack)
    int a = 10;
    int b = 20;

    /**********************************************************
        EXPERIMENTO 5
    ***********************************************************/

    for (int i = 0; i < 5; i++) {
        cout << "Iteración " << i << endl;
        funcionSinStatic();
        funcionConStatic();
    }

    /********************************************************/

    return 0;
}
```
- cada vez que se imprime la funcion `funcionSinStatic()` siempre da el valor de 100 mientras que `funcionConStatic()`, cada vez que se itera se le está sumando +1
- esto pasa porque `var_no_estatica` se crea desde cero cada vez que entras a funcionSinStatic(), Entonces siempre empieza en 100, la incrementas, pero al salir de la función se pierde ese cambio. Y `var_estatica` se crea una sola vez (la primera vez que llamas funcionConStatic()) y luego recuerda su valor, así que cada vez que vuelves a entrar, continúa desde donde quedó.

¿Ves alguna diferencia entre locales estáticas y no estáticas?
- Las locales no estática se “reinicia” en cada llamada y las locales estática conserva su valor entre llamadas.

- Experimento 6: modificar el segmento de heap

``` c++
#include <iostream>
using namespace std;

int main() {
    // Tamaño del arreglo dinámico
    int tam = 5;

    // Asignar memoria en el Heap para un arreglo de enteros
    int* arrayHeap = new int[tam];

    // Inicializar y mostrar los valores y direcciones de memoria
    for (int i = 0; i < tam; i++) {
        arrayHeap[i] = (i + 1) * 10;
        cout << "arrayHeap[" << i << "] = " << arrayHeap[i]
            << " en dirección " << (arrayHeap + i) << endl;
    }

    // Liberar la memoria asignada en el Heap
    delete[] arrayHeap;

    /**********************************************************
        EXPERIMENTO 6
    ***********************************************************/

    cout << arrayHeap[0] << endl;


    /********************************************************/


    return 0;
}
```

- suelta error porque se intenta usar un puntero que no tiene contenido ya que inmediatamente antes fue eliminado

¿Qué diferencias notas entre el comportamiento y la gestión del Heap en comparación con el Stack?

- Stack: las variables se crean y se borran solas al entrar/salir de funciones (automático, rápido).
- Heap: tú pides memoria con new y tú mismo debes devolverla con delete (manual). Si la liberas y la vuelves a usar, pasan errores como este.

¿Qué consecuencias tendría no liberar la memoria reservada con new?

- Se produce una fuga de memoria, provocando que el software consuma cada vez más recursos con el tiempo. Esto suele derivar en un rendimiento lento, inestabilidad o el bloqueo del 		sistema  

¿Por qué es importante usar delete[] al liberar memoria asignada para un arreglo?
- Porque `new int[tam]` reserva un arreglo (varios elementos), delete[] le dice al sistema: “voy a liberar todo el arreglo correctamente”.
Si usas delete en vez de `delete[]`, estás liberando “como si fuera un solo elemento” y no todo el conjunto
	
### Actividad 05

Explica qué ocurre al copiar un objeto en C++ y en C#. ¿Qué diferencias encuentras?

- en C++ se crea una nueva direccion con el mismo contenido del de la original dentro del stack permitiendo su uso independiente teniendo como base el contenido de la original. En cambio, en C#, se crea un puntero con la direccion del objeto creado en el stack y este ultimo, el objeto, se guarda en el heap, y, al crear la copia de esa forma, lo que hace es guardar la informacion que contiene original (el puntero), o sea la direccion del objeto y no el contenido de dicha direccion, finiquitando asi con 2 punteros hacia el mismo objeto

¿Qué es copia en C++ y en C#? ¿Es una copia independiente de original?

- en C++ es lo que su nombre lo indica, y en C# es un nuevo puntero
- en C++ si, en C# no

### Actividad integradora de investigación

Código
	
``` c++
#include <iostream>

int contador_global = 100;

void ejecutarContador() {
    static int contador_estatico = 0;
    contador_estatico++;
    std::cout << "  -> Llamada a ejecutarContador. Valor de contador_estatico: " << contador_estatico << std::endl;
}

void sumaPorValor(int a) {
    a = a + 10;
    std::cout << "  -> Dentro de sumaPorValor, 'a' ahora es: " << a << std::endl;
}

void sumaPorReferencia(int& a) {
    a = a + 10;
    std::cout << "  -> Dentro de sumaPorReferencia, 'a' ahora es: " << a << std::endl;
}

void sumaPorPuntero(int* a) {
    *a = *a + 10;
    std::cout << "  -> Dentro de sumaPorPuntero, '*a' ahora es: " << *a << std::endl;
}

int main() {
    int val_A = 20;
    int val_B = 20;
    int val_C = 20;

    std::cout << "--- Experimento con paso de parámetros ---" << std::endl;
    std::cout << "Valor inicial de val_A: " << val_A << std::endl;
    sumaPorValor(val_A);
    std::cout << "Valor final de val_A: " << val_A << std::endl << std::endl;

    std::cout << "Valor inicial de val_B: " << val_B << std::endl;
    sumaPorReferencia(val_B);
    std::cout << "Valor final de val_B: " << val_B << std::endl << std::endl;

    std::cout << "Valor inicial de val_C: " << val_C << std::endl;
    sumaPorPuntero(&val_C);
    std::cout << "Valor final de val_C: " << val_C << std::endl << std::endl;

    std::cout << "--- Experimento con variables estáticas ---" << std::endl;
    ejecutarContador();
    ejecutarContador();
    ejecutarContador();

    return 0;
}
```


#### A) Predicción (sin ejecutar el código)

1. ¿Cuál será la salida final en la consola de este programa?

	- para `val_A` termina con valor de 20 porque se cambia es una copia y las otras 2 (`val_B` Y `val_C`) terminan con 30 porque se hacen por referencia y por puntero lo que cambian la original. Y se imprime `contador_estatico` con 1, 2 y 3

2. Escribe la salida completa que esperas.
	
	- 
	
3. Dibuja un mapa de memoria conceptual de este programa

	- SEGMENTO DE CÓDIGO

   		- main()
       	- ejecutarContador()
       	- sumaPorValor(int)
       	- sumarPorReferencia(int&)
       	- sumarPorPuntero(int*)
  
    - DATOS GLOBALES / ESTATICOS
  
    	- contador_global (global)
   		- contador_estatico (static dentro de una funcion)
  
	- HEAP
 
		- no hay (no se usa `new`)

	- STACK 
  
		- val_A
  		- val_B
    	- val_C 


#### B) Verificación y análisis (usando el depurador)

4. 


5. 


6. Explica con tus propias palabras el comportamiento de contador_estatico. ¿Por qué “recuerda” su valor entre llamadas a la función ejecutarContador? ¿En qué se diferencia de una variable local normal?

	- `contador_estatico` recuerda porque se le declara como estatico, se crean solo una vez y estos quedan guardados en la seccion de globales y estaticos, lo que perduran al cambiar de funciones (a diferencia de las variables locales normales que se crean al entrar y se borran al salir de las funciones), lo que permiten acumular cambios y duran hasta que termina el programa


### Actividad 06

Código en C++

``` c++
#include <iostream>
using namespace std;

class Punto {
public:
    int x;
    int y;

    // Constructor
    Punto(int _x, int _y) : x(_x), y(_y) {
        cout << "Constructor: Punto(" << x << ", " << y << ") creado." << endl;
    }

    // Destructor
    ~Punto() {
        cout << "Destructor: Punto(" << x << ", " << y << ") destruido." << endl;
    }

    // Método para imprimir valores
    void imprimir() {
        cout << "Punto(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    // Coloca un breakpoint en la siguiente línea
    Punto p(10, 20);

    // Muestra el contenido del objeto
    p.imprimir();

    // Utiliza el depurador para inspeccionar 'p', observa la dirección de memoria y el valor de x e y.
    return 0;
}
```

Codigo en C#

``` c#
using System;

public class Punto
{
    public int x;
    public int y;

    public Punto(int _x, int _y)
    {
        x = _x;
        y = _y;
        Console.WriteLine($"Constructor: Punto({x}, {y}) creado.");
    }

    ~Punto()
    {
        Console.WriteLine($"Destructor: Punto({x}, {y}) destruido.");
    }

    public void Imprimir()
    {
        Console.WriteLine($"Punto({x}, {y})");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Punto p = new Punto(10, 20);
        p.Imprimir();
    }
}
```


**Abre la calculadora de Windows y selecciona el modo de programador. Cambia a modo hexadecimal. Escribe 0a ¿Qué valor en decimal obtienes? Escribe 14 ¿Qué valor en decimal obtienes? ¿Qué observas?**

0a = 10, 14 = 20, se obsera que los valores de x y y están almacenados directamente en memoria como enteros.

**Si la arquitectura de tu computador fuera big-endian, ¿Cómo quedarían almacenados los bytes en la memoria de p?**

quedaria asi: 00 00 00 0a 00 00 00 14

#### Reflexiona sobre las siguientes cuestiones:

1. ¿Cuál es la diferencia entre un constructor y un destructor en C++?
   
- Constructor: se ejecuta cuando el objeto se crea. Inicializa los datos.
- Destructor: se ejecuta cuando el objeto se destruye (en este caso, cuando termina main). Libera recursos si es necesario.

2. ¿Cuál es la diferencia entre un objeto y una clase en C++?

Clase es el molde o plantilla (define qué tendrá el objeto), y objeto es la instancia real creada a partir de la clase.

3. ¿Qué diferencia notas entre el objeto Punto en C++ y C#?

en C++ esta almacenada en el stack y es el objeto mismo, en C#, p no es el objeto sino una referencia, el objeto real esta en el heap

4. ¿Qué es p en C++ y qué es p en C#?

en C++, `p` es un objeto y en C#, `p` es una referencia a un objeto

5. ¿En qué parte de memoria se almacena p en C++ y en C#?

en C++, `p` esta en el stack y en C#, `p` esta en el heap

6. ¿Qué observaste con el depurador acerca de p? Según lo que observaste ¿Qué es un objeto en C++?

El depurador muestra que:
- p ocupa espacio real en memoria.
- Sus datos están almacenados directamente de forma contigua.
- No es un puntero ni una referencia.
- Es simplemente una estructura de datos con sus miembros almacenados uno después del otro.
   
### Actividad 07

Código

``` c++
#include <iostream>
using namespace std;

class Punto {
public:
    int x;
    int y;

    // Constructor
    Punto(int _x, int _y) : x(_x), y(_y) {
        cout << "Constructor: Punto(" << x << ", " << y << ") creado." << endl;
    }

    // Destructor
    ~Punto() {
        cout << "Destructor: Punto(" << x << ", " << y << ") destruido." << endl;
    }

    // Método para imprimir valores
    void imprimir() {
        cout << "Punto(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    // Objeto en el stack
    Punto pStack(30, 40);
    pStack.imprimir();

    // Objeto en el heap
    Punto* pHeap = new Punto(50, 60);
    pHeap->imprimir();

    // Coloca breakpoints en la creación de pStack y pHeap
    // Inspecciona las direcciones de memoria de ambos objetos:
    // - pStack: dirección obtenida directamente.
    // - pHeap: la variable pHeap es un puntero que contiene la dirección del objeto en el heap.

    // Recuerda liberar la memoria del heap
    delete pHeap;

    return 0;
}
```


1. Explicación de la diferencia entre objetos creados en el stack y en el heap.

| Stack | Heap|
|-------|-----|
| `Punto pStack (30, 40);` | `Punto* pHeap = new Punto(50, 60);` |
| se crea autamaticamente | se crea manualmente usando `new` |
| vive solo dentro de `main` | vive hasta que se borre con `delete` |
|cuando `main` termina se llama el destructor | si no se hace `detele`, solo queda ocupando memoria |
| su direccion se obtiene con `&pStack` | `pHeap` guarda la direccion del objeto real que esta en el heap |

2. `pStack` ¿Es un objeto o una referencia a un objeto?

`pStack` es un objeto que esta guardado directamente en el stack

3. `pHeap` ¿Es un objeto o una referencia a un objeto? Si es una referencia, ¿A qué objeto hace referencia?

`pHeap` es una referencia / puntero a un objeto, hace referencia referencia al objeto Punto(50,60) que está en el heap

4. ¿Qué observas en el depurador?

en locals, `pHeap` se muestra algo como 0x00000162f6545160{x=50 y=60}
En Memory1, al mirar &pHeap, lo que aparece guardado ahí es 60 51 54 f6 62 01 00 00, lo que seria la dirección del objeto
en little endidan seria 00 00 01 62 f6 54 51 60, lo que en hexadecimal seria 00 00 01 62 f6 54 51 60, Exactamente el valor que tenía pHeap en locals


### Actividad 08

Código

``` c++
#include <iostream>
#include <string>
using namespace std;

class Punto {
public:
  string name;
    int x;
    int y;

    // Constructor
    Punto(string _name, int _x, int _y) : name(_name),x(_x), y(_y) {
        cout << "Constructor: Punto "<< name <<" (" << x << ", " << y << ") creado." << endl;
    }

    // Destructor
    ~Punto() {
        cout << "Destructor: Punto " << name << "(" << x << ", " << y << ") destruido." << endl;
    }

    // Método para imprimir valores
    void imprimir() {
        cout << "Punto "<< name << "(" << x << ", " << y << ")" << endl;
    }
};

void cambiarNombre(Punto p, string nuevoNombre) {
  p.name = nuevoNombre;
}

int main() {
    // Objeto original
    Punto original("original",70, 80);
    original.imprimir();
    cambiarNombre(original, "cambiado");
    original.imprimir();
    return 0;
}
```


1. ¿Qué ocurre después de llamar a la función cambiarNombre? ¿Por qué aparece el mensaje Destructor: Punto cambiado(70, 80) destruido.? ¿Por qué original sigue existiendo luego de llamar cambiarNombre?

cuando se llama la funcion `cambiarNombre(original, "cambiado")` no se esta cambiando el objeto original, lo que pasa es que la funcion recibe el parametro asi: `void cambiarNombre(Punto p, string nuevoNombre)`, esto significa que `p` es una copia del objeto original, entonces dentro de la funcion cambias el nombre de la copia y no del original
aparece el mensaje del destructor porque la copia `p` existe solo mientras la funcion esta ejecutandose, asi que cuando la funcion termina, `p` se destruye automaticamente y como en ese momento `p.name`  ya era "cambiado" el destructor imprime Destructor: Punto cambiado(70, 80) destruido.

2. ¿En qué parte del mapa de memoria está original y en cuál está p? ¿Son el mismo objeto?

`original` es una variable local de `main` o sea que esta en el stack, `p` es un parametro local de `cambiarNombre` o sea que tambien esta en el stack pero de dicha funcion.
no son el mismo objeto, son dos distintos con el mismo contenido

Modifica la funcion cambiarNombre
``` c++ 
void cambiarNombre(Punto& p, string nuevoNombre) {
  p.name = nuevoNombre;
}
```

¿Qué ocurre ahora? ¿Por qué?

Ahora sí cambia el nombre del objeto `original`, ocurre porque ahora el parametro es `Punto& p` eso significa paso por referencia, no se crea una copia
<br>

**Modifica ahora a cambiarNombre y a main de la siguiente manera:**

Código
``` c++
void cambiarNombre(Punto* p, string nuevoNombre) {
  p->name = nuevoNombre;
}

int main() {
    // Objeto original
    Punto original("original",70, 80);
    original.imprimir();

    cambiarNombre(&original, "cambiado");
    original.imprimir();

    return 0;
}
```

**¿Qué ocurre ahora? ¿Por qué?**

Ahora sí cambia el nombre de original, antes de llamar la función: imprime `original`, después de `cambiarNombre(&original, "cambiado")`: imprime `cambiado`, y ya no aparece el destructor “extra” en medio, porque no se creó una copia
Esto para porque estás pasando la dirección de `original` con `&original`. Entonces p apunta al objeto real, y con `p->name = nuevoNombre;` esta modificando directamente el mismo objeto en memoria

**En este caso ¿Cuál es la diferencia entre pasar un objeto por valor, por referencia y por puntero?**

- Por valor se crea una copia del objeto, los cambios solo afectan a la copia y al salir de la funcion, esa copia se destruye
- Por referencia se esta manejando el mismo objeto pero con otro nombre local, los cambios se quedan en el original
- Por puntero, se pasa una direccion y el parametro es el puntero, los cambios se quedan en el original


### Actividad 09

1. ¿Qué puedes concluir de los miembros estáticos y de instancia de una clase en C++? ¿Cómo se gestionan en memoria? ¿Qué ventajas y desventajas tienen? ¿Cuándo es útil utilizarlos?

**Miembros de instancia ej: valor**

- cada objeto tiene su propia copia, si cambias `c1.valor`, no afecta `c2.valor`
- en memoria van dentro del objeto, o sea, estan donde este alamacenado ese objeto, stack si el objeto es local, heap si fue creado con `new`

ventajas

- datos independientes por objeto
- mas facil de razonar, cada objeto controla su estado

desventajas
- si tienes muchos objetos, cada uno ocupa su propio espacio

cuando se usa
- casi siempre, para representar el estado propio de cada instancia

***Mienbros estaticos ej: total**

- existen una sola vez para toda la clase, no por objeto
- todos los objetos comparten ese mismo total
- en memoria se guarda en la zona de datos globales/estaticos, separado de cada objeto

ventajas

- util para informacion compartida, contadores, configuracion global

desventajas

- es como un global de la clase, si muchos lugares lo modifican, puede volverse confuso
- puede generar dependencias y efectos secundarios

cuando se usa

- cuando necesitas un dato comun a todas las instancias: contador de instancias, caches compartidas, configuracion, etc

¿En qué segmento de memoria están c1, c2, c3 y Contador::total?

`c1` y `c2` estan en el stack, porque son variables locales creadas dentro de main
`contador::total` estan en datos globales / estaticos
`c3` (puntero) esta en el stack porque es una variable local de `main` pero el objeto creado con `new` esta en el heap

### Actividad 10

Código

``` c++
#include <iostream>
using namespace std;

class Punto {
public:
    int x;
    int y;

    // Constructor
    Punto(int _x, int _y) : x(_x), y(_y) {
        cout << "Constructor: Punto(" << x << ", " << y << ") creado." << endl;
    }

    // Destructor
    ~Punto() {
        cout << "Destructor: Punto(" << x << ", " << y << ") destruido." << endl;
    }

    // Método para imprimir valores
    void imprimir() {
        cout << "Punto(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    {
        cout << "Inicio del bloque" << endl;
        Punto pBloque(100, 200);
        // Coloca un breakpoint aquí para ver 'pBloque' en el stack.
        pBloque.imprimir();
    }
    // Al salir del bloque, el destructor de 'pBloque' se invoca.
    cout << "Fuera del bloque" << endl;

    // Creación dinámica:
    Punto* pDinamico = new Punto(300, 400);
    pDinamico->imprimir();
    // 'pDinamico' sigue existiendo hasta que se libere manualmente.
    // Coloca un breakpoint aquí y observa la dirección de memoria.
    delete pDinamico;
    // Después de 'delete', el destructor se llama y la memoria se libera.

    return 0;
}
```

1. Explica el ciclo de vida de un objeto en el stack versus uno en el heap.

los objetos en stack se crean automaticamente cuando el programa entra al bloque, en el momento en que se ejecuta `Punto pBloque(100, 200)` se llama el constructor y cuando sale del bloque se llama al destructor automaticamente, es decir, en stack, el objeto vive solo mientras este dentro del bloque o funcion
los objetos en el heap se crean con `new`, manualmente, cuando se ejecuta `new Punto(300, 400);`, se llama al constructor y el objeto queda guardado en heap, aunque se salga del bloque o funcion, no se destruye automaticamente, hay que usar `delete` para llamar al destructor y asi poder liberar memoria

2. ¿Compila? ¿Por qué ocurre esto?

no, no compila. Porque `pBloque2` fue declarada dentro del bloque y se esta usando fuera del bloque. Una variable declarada dentro de un bloque solo existe dentro de esas llaves, cuando se sale del bloque la variable ya no es visible y el compilador no sabe a que se refiere. Se pierde la variable puntero

3. Modifica el programa para declarar pBloque2 por fuera del bloque, pero inicializarlo dentro del bloque. ¿Qué ocurre? ¿Por qué?

Código modificado

``` c++
#include <iostream>
using namespace std;

class Punto {
public:
    int x;
    int y;

    // Constructor
    Punto(int _x, int _y) : x(_x), y(_y) {
        cout << "Constructor: Punto(" << x << ", " << y << ") creado." << endl;
    }

    // Destructor
    ~Punto() {
        cout << "Destructor: Punto(" << x << ", " << y << ") destruido." << endl;
    }

    // Método para imprimir valores
    void imprimir() {
        cout << "Punto(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    {
        cout << "Inicio del bloque" << endl;
        Punto pBloque(100, 200);
        pBloque.imprimir();
        // Coloca un breakpoint aquí para ver 'pBloque' en el stack.
    }
    // Al salir del bloque, el destructor de 'pBloque' se invoca.
    cout << "Fuera del bloque" << endl;
    // Creación dinámica:
    Punto* pDinamico = new Punto(300, 400);
    pDinamico->imprimir();
    // 'pDinamico' sigue existiendo hasta que se libere manualmente.
    // Coloca un breakpoint aquí y observa la dirección de memoria.
    delete pDinamico;
    // Después de 'delete', el destructor se llama y la memoria se libera.

    Punto* pBloque2 = nullptr;   // declarada fuera

    {
        cout << "Inicio del bloque 2" << endl;
        pBloque2 = new Punto(500, 600);  // inicializada dentro
        pBloque2->imprimir();
    }

    // aquí sí existe la variable pBloque2 (el puntero)
    pBloque2->imprimir();
    delete pBloque2;
    pBloque2 = nullptr;

    return 0;
```

ahora si el compila y corre el programa, porque ahora `pBloque2` fue declarada en el alcance de `main`, entonces sigue existinedo despues del bloque y como el objeto fue creado con `new`, ese objeto vive en el heap y no se destruye al salir del bloque, solo si se hace `detele` a `pBloque2`
<br>

En este caso

``` c++
#include <iostream>
using namespace std;

class Punto {
public:
    int x;
    int y;

    // Constructor
    Punto(int _x, int _y) : x(_x), y(_y) {
        cout << "Constructor: Punto(" << x << ", " << y << ") creado." << endl;
    }

    // Destructor
    ~Punto() {
        cout << "Destructor: Punto(" << x << ", " << y << ") destruido." << endl;
    }

    // Método para imprimir valores
    void imprimir() {
        cout << "Punto(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    {
        cout << "Inicio del bloque" << endl;
        Punto pBloque(100, 200);
        // Coloca un breakpoint aquí para ver 'pBloque' en el stack.
        pBloque.imprimir();
    }

    Punto* pBloque2 = nullptr;
    {
        cout << "Inicio del bloque 2" << endl;
        pBloque2 = new Punto(500, 600);
        pBloque2->imprimir();
    }
    pBloque2->imprimir();
    delete pBloque2;

    return 0;
}
```

4. ¿Por qué el objeto pBloque se destruye al salir del bloque y pBloque2 no?
Recuerda de nuevo, pBloque2 es un objeto o es una referencia a un objeto?

`pBloque` fue creado como un objeto normal en el stack, entonces cuando el programa sale del bloque, ese objeto deja de existir automaticamente y por eso se llama al destructor. En cambio `pBloque2` fue creado usando `new` el cual crea el objeto en el heap por lo que no se destruye al salir del bloque y no se pierde por haberse declarado el puntero fuera de este

5. ¿En qué parte de la memoria se almacena pBloque2?

en el stack del `main`

6. ¿En qué parte de la memoria se almacena el objeto al que apunta pBloque2?

en el heap

## Bitácora de aplicación 

### Actividad Integradora

1. Diagnóstico del problema

- Error 1: La clase del constructor hace: `estadisticas = new int[3];` pero aquel espacio de la memoria no se libera. Esto ocurre porque el heroe es un objeto local que vive en el stack, `estadisticas` apunta a un arreglo que vive en el heap y al salir de `simularEncuentro`, el heroe desaparece del stack pero el arreglo del heap seguirá ahí ya que no fué liberado. Este error lleva a que a lo largo del juego, mientras mas se sumen encuentros, la memoria seguirá ocupandose hasta que crashee porque no va a ser liberado el espacio de esta.
  
- Error 2: La copia: `Personaje copiaHeroe = heroe;` está mal hecha, ya que hace una copia la informacion que contienetambien el puntero de `estadisticas` tal cual en vez de copiar el valor de cada estadistica. Ocurre porque en el stack quedan dos objetos distinto: `heroe` y `copiaHeroe` pero en el heap no se crean otras estadisticas sino que ambos apuntan al mismo bloque. Esto hace que si se intentara cambiar las stats de uno de los objetos, el otro tendría el mismo cambio.


2. USA EL DEPURADOR

Error 1
<img width="941" height="690" alt="image" src="https://github.com/user-attachments/assets/42cca1cb-5098-4290-bfc4-f23046d9054b" />
El encuentro finalizó y el espacio de memoria no fué liberado

Error 2
<img width="940" height="721" alt="image" src="https://github.com/user-attachments/assets/22a2d246-5872-4cf6-9bd2-c0cedabc5d7f" />
Tienen la misma direccion para estadisticas

3. Induce los fallos

- Error 1: haciendo esta modificación

<img width="596" height="84" alt="image" src="https://github.com/user-attachments/assets/28642323-41b2-4cfb-99f2-3a9daf2a981c" />

Al inicio
<img width="746" height="568" alt="image" src="https://github.com/user-attachments/assets/36ccd33b-1128-4482-bfbd-edb2b3c9beb6" />

Despues de algunos segundos
<img width="752" height="565" alt="image" src="https://github.com/user-attachments/assets/1a2d88b9-3037-435e-bddc-7428d21c964b" />

- Error 2: agregando esto despues de crear la copia

<img width="714" height="175" alt="image" src="https://github.com/user-attachments/assets/67292b69-5f85-411c-8dbf-cb4ade073909" />

DEMOSTRACIÓN:
<img width="563" height="81" alt="image" src="https://github.com/user-attachments/assets/16852d99-84d1-4963-98de-4efad2fd87ef" />


4. Solución y refactorización (síntesis y creación)

Reemplazar el `int* estadisticas;` por un array de tres espacios que permita guardar los datos dentro del objeto y así evitar fugas de memoria. (recordar agregar el include de arrays)


Refactorizando

``` c++
#include <iostream>
#include <string>
#include <array>

class Personaje {
public:
    std::string nombre;
    std::array<int, 3> estadisticas; 

    Personaje(const std::string& n, int vida, int ataque, int defensa)
        : nombre(n), estadisticas{vida, ataque, defensa} {
        std::cout << "Constructor: nace " << nombre << std::endl;
    }

    void imprimir() const {
        std::cout << "Personaje " << nombre
            << " [Vida: " << estadisticas[0]
            << ", ATK: " << estadisticas[1]
            << ", DEF: " << estadisticas[2]
            << "]" << std::endl;
    }
};
```

para el error 2, seria eliminar el puntero dinamico y usar un cotenedor seguro:

``` c++
std::array<int, 3> estadisticas;
```

ahora cuando se hace la copia del heroe, se copia el nombre, los 3 enteros de estadisticas y cada objeto tiene su propio almacenamiento independiente

Código modificado

``` c++
#include <iostream>
#include <string>
#include <array>

class Personaje {
public:
    std::string nombre;
    std::array<int, 3> estadisticas; // [vida, ataque, defensa]

    // Constructor (sin new, sin delete)
    Personaje(const std::string& n, int vida, int ataque, int defensa)
        : nombre(n), estadisticas{ vida, ataque, defensa } {
        std::cout << "Constructor: nace " << nombre << std::endl;
    }

    // (No hace falta destructor: no hay memoria dinámica manual)
    // (No hace falta constructor de copia ni operador de asignación:
    //  std::string y std::array copian correctamente)

    void imprimir() const {
        std::cout << "Personaje " << nombre
            << " [Vida: " << estadisticas[0]
            << ", ATK: " << estadisticas[1]
            << ", DEF: " << estadisticas[2]
            << "]" << std::endl;
    }
};

void simularEncuentro() {
    std::cout << "\n--- Iniciando encuentro ---" << std::endl;

    Personaje heroe("Aragorn", 100, 20, 15);
    heroe.imprimir();

    // Ahora la copia es segura (copia real de estadisticas)
    Personaje copiaHeroe = heroe;
    copiaHeroe.nombre = "Copia de Aragorn";

    copiaHeroe.imprimir();

    std::cout << "Saliendo del encuentro..." << std::endl;
}

int main() {
    simularEncuentro();
    std::cout << "\nSimulación terminada." << std::endl;
    return 0;
}
```

5. Justificación de la Solución

Cambio 1: Reemplazar int* estadisticas por std::array<int, 3> estadisticas

Con int*, el arreglo se creaba en el heap con new, pero como no había liberación, el bloque quedaba ocupado incluso después de destruir el objeto.
Con std::array, las estadísticas ya no están en el heap, sino que quedan almacenadas directamente dentro del objeto.
Por lo tanto, ya no se reserva memoria dinámica, y no existe nada que liberar manualmente.
Resultado: no hay fugas de memoria, incluso si se crean miles de personajes.
¿Por qué esto resuelve la copia superficial?
Cuando estadisticas era un puntero, copiar un objeto copiaba solo la dirección, no los datos.
Con std::array, al copiar un Personaje, se copia el contenido real del arreglo (los 3 enteros).
Resultado: heroe y copiaHeroe tienen estadísticas independientes, evitando bugs donde uno cambia y el otro también.

Cambio 2: Eliminar new y delete de la clase

new/delete manuales son una fuente común de errores en C++ (fugas, doble liberación, punteros colgantes).
Al eliminarlos, el ciclo de vida de la memoria queda manejado automáticamente por objetos seguros de la STL (std::string, std::array).
Esto evita crashes impredecibles por corrupción del heap.

## Bitácora de reflexión



# Unidad 4

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
### Evidencia 1 (inserción del primer nodo en una cola vacía (enqueue))
<img width="940" height="691" alt="image" src="https://github.com/user-attachments/assets/2e137dc6-962b-4a39-9fe0-355496d52977" />

- Variables y valores específicos
  - newNode: Contiene una dirección de memoria válida (0x000001816387d2c0), lo que indica que se creó correctamente un nuevo nodo con los datos del trazo.
  - this: Representa la instancia actual de BrushQueue.
  - front y rear: No apuntan a ningún nodo, por lo tanto la cola está vacía.
  - Demás variables: Fueron atributos de prueba para hacer correr el depurador, pero se demuestra que fueron creadas exitosamente con las propiedades dadas.

- Justificación
  Esta evidencia demuestra que la función enqueue() detecta correctamente cuando la cola está vacía mediante la función isEmpty().
  El depurador muestra que tanto front como rear tienen el valor NULL, lo que confirma que no existen nodos en la estructura antes de la inserción.
  Debido a esta condición, el algoritmo ejecutará las instrucciones:

``` C++
front = newNode;
rear = newNode;
```
  lo cual establece correctamente el primer nodo de la cola. Esto garantiza que la estructura FIFO queda inicializada correctamente y que futuras inserciones podrán agregarse al final utilizando el puntero rear.


### Evidencia 2 (mantenimiento del orden FIFO al insertar más nodos (enqueue))
<img width="939" height="518" alt="image" src="https://github.com/user-attachments/assets/eccc189a-6f49-41a7-a166-88fb1c03e636" />

- Variables y valores específicos
  - newNode: Contiene la dirección de memoria 0x0000015e82ec1db0, lo que indica que se creó correctamente un nuevo nodo con los datos del trazo (posición x = 330, y = 269, radio 10, etc.). Este nodo es el que se insertará al final de la cola.

  - rear: Contiene la dirección de memoria 0x0000015e82c65fa0, lo que indica que actualmente apunta al último nodo existente en la cola antes de la inserción del nuevo nodo.

  - rear->next: Contiene la dirección de memoria 0x0000015e82ec1db0, lo que indica que el campo next del nodo apuntado por rear ahora enlaza con el nuevo nodo creado. Esto demuestra que el nuevo nodo fue conectado correctamente al final de la cola.

  - this: Representa la instancia actual de la estructura BrushQueue, ubicada en la dirección de memoria 0x0000015e81e0b2d8. A través de esta referencia se puede observar el estado actual de la cola y acceder a sus atributos internos.

  - front: Contiene la dirección de memoria 0x0000015e82c65fa0, lo que indica que sigue apuntando al primer nodo insertado en la cola. Esto confirma que la inserción de un nuevo nodo no altera el nodo inicial de la estructura.

  - size: Tiene el valor 1, lo que indica que antes de completar la operación de inserción la cola registraba un elemento.

  - maxSize: Tiene el valor 50, lo que indica que la cola puede almacenar hasta 50 nodos antes de que se active la eliminación automática mediante dequeue().

- Justificacion
  Esta evidencia demuestra que la operación enqueue() mantiene correctamente el orden FIFO de la cola. El nuevo nodo se inserta siempre al final de la estructura enlazada mediante el puntero rear->next. Posteriormente, el puntero rear se actualiza para señalar el nuevo último nodo de la cola.

  Este mecanismo garantiza que los elementos se agregan en orden de llegada y que el primer nodo insertado permanece en la posición frontal de la cola hasta que se ejecute la operación dequeue().

### Evidencia 3 (comportamiento de eliminación y prevención de fugas (dequeue))

(No me está dejando subir capturas a partir de aquí)

- Variables y valores específicos
  - temp: Contiene la dirección de memoria del nodo apuntado originalmente por front. Este nodo corresponde al elemento más antiguo de la cola y se almacena temporalmente para poder eliminarlo correctamente después de actualizar la estructura.
  - front: Inicialmente apuntaba al primer nodo de la cola. Tras ejecutar la instrucción front = front->next, este puntero se actualiza para señalar el siguiente nodo de la estructura. Si existían más nodos, ahora apunta al que ocupaba la segunda posición; si solo existía un nodo, pasa a tener el valor NULL, indicando que la cola queda vacía.
  - front->next: Representa el nodo que seguirá al nuevo front dentro de la cola. Este valor permite verificar que la estructura enlazada continúa siendo válida después de la eliminación del nodo más antiguo.
  - rear: Continúa apuntando al último nodo de la cola y no se modifica durante esta operación, ya que la eliminación ocurre únicamente en el inicio de la estructura. Solo cambiaría si la cola quedara completamente vacía.
  - size: Disminuye en una unidad después de ejecutar size--, lo que refleja correctamente que el número total de nodos almacenados en la cola ha sido reducido.
  - this: Representa la instancia actual del objeto BrushQueue, desde donde se puede observar el estado interno de la cola antes y después de la eliminación.

- Justificación
  Esta evidencia demuestra que la función dequeue() elimina correctamente el nodo más antiguo de la cola, cumpliendo con el principio FIFO (First In, First Out). Al actualizar el puntero front hacia el siguiente nodo, la estructura conserva el orden de los elementos restantes, permitiendo que el siguiente nodo insertado pase a ocupar la primera posición.

  Además, el nodo eliminado se libera explícitamente mediante la instrucción delete temp, lo que garantiza que la memoria dinámica utilizada por dicho nodo no permanezca reservada. Esto previene fugas de memoria y confirma que la implementación gestiona adecuadamente los recursos del sistema.

  Si la cola queda vacía tras la eliminación, los punteros front y rear se establecen en NULL, asegurando que la estructura permanezca en un estado consistente.

### Evidencia 4 (control del tamaño máximo de la cola (maxSize))

(sigo sin poder subir los pantallazos)

- Variables y valores específicos
  - size: Tiene un valor mayor que maxSize, lo que indica que la cola ha superado el número máximo de nodos permitido tras la inserción de un nuevo elemento.
  - maxSize: Representa el límite máximo de nodos que puede almacenar la cola. Este valor se mantiene constante durante la operación y sirve como referencia para determinar cuándo debe eliminarse un nodo antiguo.
  - front: Apunta al nodo más antiguo de la cola, el cual será eliminado mediante la función dequeue() para reducir el tamaño de la estructura.
  - rear: Apunta al nodo más reciente insertado, que permanece en la cola y no se ve afectado por la eliminación del nodo frontal.
  - this: Representa la instancia actual del objeto BrushQueue, desde donde se puede observar el estado completo de la estructura en el momento en que se detecta el exceso de tamaño.

- Justificacion
  Esta evidencia demuestra que la cola implementa correctamente un control de tamaño máximo. Cuando el número de nodos almacenados supera el valor de maxSize, se ejecuta automáticamente la función dequeue(), eliminando el nodo más antiguo de la estructura.

  Este mecanismo garantiza que la cola mantenga un tamaño constante y evita un crecimiento ilimitado que podría provocar un consumo excesivo de memoria. Además, al eliminar siempre el nodo frontal, se conserva el principio FIFO, asegurando que los elementos más antiguos sean los primeros en salir.


### Evidencia 5 (Evidencia 5: recorrido de la cola sin destruirla (draw))

<img width="942" height="883" alt="1000071859" src="https://github.com/user-attachments/assets/640303b7-8934-4848-bd0a-54ea9cb02eae" />


- Variables y valores específicos
  - current: Contiene la dirección de memoria 0x00000155c738eb90, lo que indica que el puntero auxiliar está apuntando a un nodo válido de la cola y se está utilizando para recorrerla.
  - strokes.front: Apunta a la dirección 0x00000155c738eb90, indicando el primer nodo de la cola.
  - strokes.rear: Apunta a la misma dirección que front, lo que indica que la cola contiene un solo nodo en este momento.
  - strokes.size: Tiene valor 1, confirmando que hay un elemento almacenado en la cola.
  - strokes.maxSize: Tiene valor 50, correspondiente a la capacidad máxima configurada.

- Justificación
  Esta evidencia demuestra que la función draw() recorre la cola utilizando un puntero auxiliar (current) para acceder a cada nodo sin modificar la estructura original. Los punteros front y rear permanecen intactos durante el proceso, lo que confirma que los nodos no se eliminan ni se alteran.

  El acceso a los atributos del nodo (x, y, radius, color y opacity) evidencia que los datos almacenados se utilizan únicamente para renderizar los trazos en pantalla. Por lo tanto, se valida que el recorrido de la cola se realiza sin destruirla, cumpliendo con el comportamiento esperado de una estructura FIFO utilizada como contenedor de los segmentos del gusano.
## Bitácora de reflexión

### Evidencia 6 (limpieza total de la memoria (clear))
<img width="941" height="469" alt="1000071857" src="https://github.com/user-attachments/assets/fb6ea9cf-1b91-47e0-802a-fd819984ebe7" />
<img width="924" height="479" alt="1000071858" src="https://github.com/user-attachments/assets/e136ce97-25e8-4187-860c-01d16d996daf" />

- Variables y valores específicos (1)
  - key: Tiene valor 99, que corresponde al carácter 'c' en ASCII, indicando que el usuario presionó la tecla C.
  - strokes.front: Apunta a la dirección 0x00000246689b9520, correspondiente al primer nodo almacenado.
  - strokes.rear: Apunta a la dirección 0x00000246689dd3d0, correspondiente al último nodo de la cola.
  - strokes.size: Tiene valor 35, lo que indica que existen 35 nodos en memoria.
- Variables y valores específicos (2)
  - key: Sigue siendo 99 ('c'), confirmando que la acción fue provocada por la tecla C.
  - strokes.front: Tiene valor NULL, lo que indica que ya no existe ningún nodo al inicio de la cola.
  - strokes.rear: También es NULL, indicando que no queda ningún nodo al final.
  - strokes.size: Tiene valor 0, confirmando que todos los nodos fueron eliminados.

- Justificacion
  Esta evidencia demuestra que la función clear() elimina completamente todos los nodos de la cola cuando el usuario presiona la tecla C.

  En la primera captura se observa que la cola contiene múltiples elementos (size = 35) y que los punteros front y rear apuntan a nodos válidos en memoria. Tras ejecutar la instrucción strokes.clear(), la segunda captura muestra que ambos punteros pasan a ser NULL y el tamaño de la cola se reduce a 0, lo que confirma que todos los nodos fueron eliminados y la memoria dinámica asociada fue liberada.

  El hecho de que la variable key tenga el valor 99 en ambas capturas valida que esta limpieza se produjo específicamente por la acción del usuario al presionar la tecla C y no por otros mecanismos del programa, como el control de tamaño máximo.


### Código

ofApp.cpp

``` C++
#include "ofApp.h"

//--------------------------------------------------------------
void ofApp::setup() {
	ofBackground(0);
}

//--------------------------------------------------------------
void ofApp::update() {
	backgroundHue += 0.2;
	if (backgroundHue > 255) backgroundHue = 0;

	if (ofGetMousePressed()) {

		float x = ofGetMouseX();
		float y = ofGetMouseY();

		float radius = ofRandom(5, 15);

		ofColor color;
		color.setHsb(ofRandom(255), 200, 255);

		float opacity = ofRandom(100, 255);

		strokes.enqueue(x, y, radius, color, opacity);
	}
}

//--------------------------------------------------------------
void ofApp::draw() {
	// Fondo con gradiente dinámico
	ofColor color1, color2;
	color1.setHsb(backgroundHue, 150, 240);
	color2.setHsb(fmod(backgroundHue + 128, 255), 150, 240);
	ofBackgroundGradient(color1, color2, OF_GRADIENT_LINEAR);

	// Dibujar los trazos almacenados en la cola
	Node * current = strokes.front;

	while (current != nullptr) {

		ofSetColor(current->color, current->opacity);
		ofDrawCircle(current->x, current->y, current->radius);

		current = current->next;
	}
}

//--------------------------------------------------------------
void ofApp::keyPressed(int key) {
	if (key == 'c') {
		strokes.clear();
	}
	if (key == 'a') {
		if (strokes.maxSize == 50)
			strokes.maxSize = 100;
		else
			strokes.maxSize = 50;
	} else if (key == 's') {
		ofSaveScreen("screenshot.png");
	}
}

```

ofApp.h
``` C++
#pragma once
#include "ofMain.h"

// Nodo de la cola
struct Node {
	float x, y;
	float radius;
	ofColor color;
	float opacity;
	Node * next;

	Node(float _x, float _y, float _radius, ofColor _color, float _opacity)
		: x(_x)
		, y(_y)
		, radius(_radius)
		, color(_color)
		, opacity(_opacity)
		, next(nullptr) {
	}
};

// Implementación manual de una cola (FIFO)
class BrushQueue {
public:
	Node * front;
	Node * rear;
	int size;
	int maxSize;

	BrushQueue(int _maxSize);
	~BrushQueue();

	void enqueue(float x, float y, float radius, ofColor color, float opacity);
	void dequeue();
	void clear();
	bool isEmpty();
};

// Constructor
BrushQueue::BrushQueue(int _maxSize)
	: front(nullptr)
	, rear(nullptr)
	, size(0)
	, maxSize(_maxSize) { }

// Destructor
BrushQueue::~BrushQueue() {
	clear();
}

// Implementa aquí `enqueue()`
void BrushQueue::enqueue(float x, float y, float radius, ofColor color, float opacity) {

	Node * newNode = new Node(x, y, radius, color, opacity);

	if (isEmpty()) {
		front = newNode;
		rear = newNode;
	} else {
		rear->next = newNode;
		rear = newNode;
	}

	size++;

	// Si se supera el tamaño máximo, eliminar el más antiguo
	if (size > maxSize) {
		dequeue();
	}
}

// Implementa aquí `dequeue()`
void BrushQueue::dequeue() {
	if (isEmpty()) return;

	Node * temp = front;
	front = front->next;

	delete temp;
	size--;

	if (front == nullptr) {
		rear = nullptr;
	}
}

// Implementa aquí `clear()`
void BrushQueue::clear() {
	while (!isEmpty()) {
		dequeue();
	}
}

// Implementa aquí `isEmpty()`
bool BrushQueue::isEmpty() {
	return front == nullptr;
}

class ofApp : public ofBaseApp {
public:
	BrushQueue strokes; // Cola de trazos
	float backgroundHue = 0;

	ofApp()
		: strokes(50) { } // Tamaño máximo de la cola

	void setup();
	void update();
	void draw();
	void keyPressed(int key);
};

```


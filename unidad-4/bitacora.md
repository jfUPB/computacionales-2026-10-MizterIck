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

(nada que me vuelve a dejar subir pantallazos)
## Bitácora de reflexión





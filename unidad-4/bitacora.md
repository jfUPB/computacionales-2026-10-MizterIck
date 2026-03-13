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


## Bitácora de reflexión




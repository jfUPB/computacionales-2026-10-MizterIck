# Unidad 4

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
### Evidencia 1 (inserción del primer nodo en una cola vacía (enqueue))
<img width="940" height="691" alt="image" src="https://github.com/user-attachments/assets/2e137dc6-962b-4a39-9fe0-355496d52977" />

- Variables y valores específicos
  newNode: Contiene una dirección de memoria válida (0x000001816387d2c0), lo que indica que se creó correctamente un nuevo nodo con los datos del trazo.
  this: Representa la instancia actual de BrushQueue.
  front y rear: No apuntan a ningún nodo, por lo tanto la cola está vacía.
  Demás variables: Fueron atributos de prueba para hacer correr el depurador, pero se demuestra que fueron creadas exitosamente con las propiedades dadas.

- Justificación
  Esta evidencia demuestra que la función enqueue() detecta correctamente cuando la cola está vacía mediante la función isEmpty().
  El depurador muestra que tanto front como rear tienen el valor NULL, lo que confirma que no existen nodos en la estructura antes de la inserción.
  Debido a esta condición, el algoritmo ejecutará las instrucciones:

´´´ C++
front = newNode;
rear = newNode;

´´´


## Bitácora de reflexión

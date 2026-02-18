# Unidad 3

## Bitácora de proceso de aprendizaje
### Actividad 01
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

## Bitácora de aplicación 



## Bitácora de reflexión

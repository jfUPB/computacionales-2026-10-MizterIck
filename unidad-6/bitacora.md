# Unidad 6

## Bitácora de proceso de aprendizaje

### Actividad 02

1. 

## Bitácora de aplicación 

FASE 1

ofApp.cpp

``` c++
#include "ofApp.h"
#include <algorithm>

void Subject::addObserver(Observer * observer) {
	if (!observer) return;
	if (std::find(observers.begin(), observers.end(), observer) == observers.end()) {
		observers.push_back(observer);
	}
}

void Subject::removeObserver(Observer * observer) {
	if (!observer) return;
	observers.erase(std::remove(observers.begin(), observers.end(), observer), observers.end());
}

void Subject::notify(const std::string & event) {
	for (Observer * observer : observers) {
		observer->onNotify(event);
	}
}

Particle::Particle()
	: state(nullptr) {
	position = ofVec2f(ofRandomWidth(), ofRandomHeight());
	velocity = ofVec2f(ofRandom(-0.5f, 0.5f), ofRandom(-0.5f, 0.5f));
	size = ofRandom(2.0f, 5.0f);
	color = ofColor(255);

	state = new NormalState();
	state->onEnter(this);
}

Particle::~Particle() {
	if (state) {
		state->onExit(this);
		delete state;
		state = nullptr;
	}
}

void Particle::setState(State * newState) {
	if (state) {
		state->onExit(this);
		delete state;
	}
	state = newState;
	if (state) {
		state->onEnter(this);
	}
}

void Particle::update() {
	if (state) {
		state->update(this);
	}
	keepInsideWindow();
}

void Particle::draw() {
	ofPushStyle();
	ofSetColor(color);
	ofDrawCircle(position, size);
	ofPopStyle();
}

void Particle::onNotify(const std::string & event) {
	if (event == "attract") {
		setState(new AttractState());
	} else if (event == "repel") {
		setState(new RepelState());
	} else if (event == "stop") {
		setState(new StopState());
	} else if (event == "normal") {
		setState(new NormalState());
	} else if (event == "spiral") {
		setState(new SpiralState());
	}
}

void Particle::keepInsideWindow() {
	const float W = static_cast<float>(ofGetWidth());
	const float H = static_cast<float>(ofGetHeight());

	if (position.x < 0.0f) {
		position.x = 0.0f;
		velocity.x *= -1.0f;
	} else if (position.x > W) {
		position.x = W;
		velocity.x *= -1.0f;
	}
	if (position.y < 0.0f) {
		position.y = 0.0f;
		velocity.y *= -1.0f;
	} else if (position.y > H) {
		position.y = H;
		velocity.y *= -1.0f;
	}
}

void NormalState::onEnter(Particle * particle) {
	particle->velocity.set(ofRandom(-0.5f, 0.5f), ofRandom(-0.5f, 0.5f));
}

void NormalState::update(Particle * particle) {
	particle->position += particle->velocity;
}

static void steer(Particle * particle, const ofVec2f & toward, float accel, float vmax, float posScale) {
	ofVec2f dir = toward - particle->position;
	float len = dir.length();
	if (len > 1e-6f) {
		dir /= len;
		particle->velocity += dir * accel;
	}
	particle->velocity.limit(vmax);
	particle->position += particle->velocity * posScale;
}

void AttractState::update(Particle * particle) {
	ofVec2f mouse(ofGetMouseX(), ofGetMouseY());
	steer(particle, mouse, 0.05f, 3.0f, 0.2f);
}

void RepelState::update(Particle * particle) {
	ofVec2f mouse(ofGetMouseX(), ofGetMouseY());
	ofVec2f away = particle->position - mouse;
	float len = away.length();
	if (len > 1e-6f) {
		away /= len;
		particle->velocity += away * 0.05f;
	}
	particle->velocity.limit(3.0f);
	particle->position += particle->velocity * 0.2f;
}

void StopState::update(Particle * particle) {
	particle->velocity *= 0.80f;
	if (particle->velocity.lengthSquared() < 1e-4f) {
		particle->velocity.set(0.0f, 0.0f);
	}
	particle->position += particle->velocity;
}

void SpiralState::update(Particle * particle) {
	ofVec2f mouse(ofGetMouseX(), ofGetMouseY());
	ofVec2f dir = particle->position - mouse;

	float distance = dir.length();

	if (distance > 1e-6f) {
		dir.normalize();

		ofVec2f perp(-dir.y, dir.x);

		particle->velocity += perp * 0.1f;
		particle->velocity += -dir * 0.02f;
	}

	particle->velocity.limit(3.0f);
	particle->position += particle->velocity;
}

Particle * ParticleFactory::createParticle(const std::string & type) {
	Particle * particle = new Particle();
	if (type == "star") {
		particle->size = ofRandom(2.0f, 4.0f);
		particle->color = ofColor(255, 0, 0);
	} else if (type == "shooting_star") {
		particle->size = ofRandom(3.0f, 6.0f);
		particle->color = ofColor(0, 255, 0);
		particle->velocity *= 3.0f;
	} else if (type == "planet") {
		particle->size = ofRandom(5.0f, 8.0f);
		particle->color = ofColor(0, 0, 255);
	} else if (type == "comet") {
		particle->size = ofRandom(7.0f, 10.0f);
		particle->color = ofColor(255, 255, 0);
		particle->velocity *= 2.0f;
	}
	return particle;
}

ofApp::~ofApp() {
	for (Particle * p : particles) {
		removeObserver(p);
		delete p;
	}
	particles.clear();
}

void ofApp::setup() {
	ofBackground(0);
	particles.reserve(100 + 5 + 10);

	for (int i = 0; i < 100; ++i) {
		Particle * p = ParticleFactory::createParticle("star");
		particles.push_back(p);
		addObserver(p);
	}
	for (int i = 0; i < 5; ++i) {
		Particle * p = ParticleFactory::createParticle("shooting_star");
		particles.push_back(p);
		addObserver(p);
	}
	for (int i = 0; i < 10; ++i) {
		Particle * p = ParticleFactory::createParticle("planet");
		particles.push_back(p);
		addObserver(p);
	}
	for (int i = 0; i < 8; ++i) {
		Particle * p = ParticleFactory::createParticle("comet");
		particles.push_back(p);
		addObserver(p);
	}
}

void ofApp::update() {
	for (Particle * p : particles) {
		p->update();
	}
}

void ofApp::draw() {
	for (Particle * p : particles) {
		p->draw();
	}
}

void ofApp::keyPressed(int key) {
	switch (key) {
	case 's':
		notify("stop");
		break;
	case 'a':
		notify("attract");
		break;
	case 'r':
		notify("repel");
		break;
	case 'n':
		notify("normal");
		break;
	case 'p':
		notify("spiral");
		break;
	default:
		break;
	}
}

```

ofApp.h

``` c++
#pragma once

#include "ofMain.h"
#include <string>
#include <vector>

class Observer {
public:
	virtual ~Observer() = default;
	virtual void onNotify(const std::string & event) = 0;
};

class Subject {
public:
	void addObserver(Observer * observer);
	void removeObserver(Observer * observer);

protected:
	void notify(const std::string & event);

private:
	std::vector<Observer *> observers;
};

class Particle;

class State {
public:
	virtual ~State() = default;
	virtual void update(Particle * particle) = 0;
	virtual void onEnter(Particle * particle) { }
	virtual void onExit(Particle * particle) { }
};

class Particle : public Observer {
public:
	Particle();
	~Particle() override;

	Particle(const Particle &) = delete;
	Particle & operator=(const Particle &) = delete;

	void update();
	void draw();
	void onNotify(const std::string & event) override;

	void setState(State * newState);

	ofVec2f position;
	ofVec2f velocity;
	float size;
	ofColor color;

private:
	void keepInsideWindow();
	State * state;
};

class NormalState : public State {
public:
	void update(Particle * particle) override;
	void onEnter(Particle * particle) override;
};

class AttractState : public State {
public:
	void update(Particle * particle) override;
};

class RepelState : public State {
public:
	void update(Particle * particle) override;
};

class StopState : public State {
public:
	void update(Particle * particle) override;
};

class SpiralState : public State {
public:
	void update(Particle * particle) override;
};

class ParticleFactory {
public:
	static Particle * createParticle(const std::string & type);
};

class ofApp : public ofBaseApp, public Subject {
public:
	~ofApp() override;
	void setup() override;
	void update() override;
	void draw() override;
	void keyPressed(int key) override;

private:
	std::vector<Particle *> particles;
};

```

FASE 2

EVIDENCIA 1:

<img width="932" height="779" alt="image" src="https://github.com/user-attachments/assets/d73b4651-dfcf-4124-aafe-4212e095a022" />

Se colocó el breakpoint en la rama else if (type == "comet") porque es donde se crea y configura el nuevo tipo de partícula. Este punto permite verificar que el programa entra en la condición correcta y observar los valores del objeto Particle recién creado, confirmando que sus atributos (tamaño, color y velocidad) se asignan correctamente.

EXPLICACIÓN: 
En la captura se observa la ejecución del método ParticleFactory::createParticle, donde el valor del parámetro type es "comet". El flujo del programa entra correctamente en la rama correspondiente a este tipo de partícula.

Se puede inspeccionar el objeto particle, el cual ya ha sido creado dinámicamente. En este punto se observan sus atributos, incluyendo el tamaño (size), el color (color) configurado como amarillo (255,255,0) y la velocidad (velocity), la cual está siendo modificada en esta sección del código.

JUSTIFICACIÓN:
Se eligió este punto de inspección porque permite verificar directamente qué rama del condicional se ejecuta dentro de la fábrica. Además, permite observar el estado interno del objeto Particle inmediatamente después de su creación y durante su configuración.

Esto confirma que el nuevo tipo "comet" está correctamente integrado en la ParticleFactory, tanto en la selección de la rama como en la asignación de sus propiedades específicas.
## Bitácora de reflexión

EVIDENCIA 2:

Spiral
<img width="925" height="504" alt="image" src="https://github.com/user-attachments/assets/ec550329-3e9b-45c0-9ada-c19f401403cb" />

Normal
<img width="919" height="509" alt="image" src="https://github.com/user-attachments/assets/669f4338-b9f9-48ee-8153-446052cd929f" />

Se colocó el breakpoint en el método setState, después de la asignación del nuevo estado (state = newState), ya que en este punto el objeto es válido y se puede inspeccionar correctamente su _vtable.

¿Qué entradas cambian?
La principal diferencia se encuentra en la entrada de la función update(Particle*), cuya dirección de memoria es distinta en cada estado.

EXPLICACIÓN:
Al comparar ambas capturas, se observa que la _vtable contiene diferentes direcciones de funciones según el estado activo. En particular, la entrada correspondiente al método update cambia:

En NormalState, apunta a NormalState::update
En SpiralState, apunta a SpiralState::update

Esto indica que cada clase tiene su propia implementación de los métodos virtuales.

JUSTIFICACIÓN:
Las entradas de la _vtable cambian porque cada estado redefine métodos virtuales de la clase base State. Esto permite que, a través de un puntero State*, se ejecute en tiempo de ejecución la función correspondiente al tipo real del objeto.

Este comportamiento corresponde al polimorfismo dinámico.

EVIDENCIA 3:

<img width="933" height="314" alt="image" src="https://github.com/user-attachments/assets/7d264327-6b1a-4344-ae91-33086bf5514a" />
En esta captura se observa la ejecución del método keyPressed al presionar la tecla 'p'. El programa se detiene justo en la llamada a notify("spiral"), lo que indica que se está generando el evento que será enviado a todos los observadores.

<img width="941" height="651" alt="image" src="https://github.com/user-attachments/assets/3d1ab255-7264-4db0-a50e-5f4a95a88eed" />
En esta captura se observa la ejecución del método onNotify en una partícula. El valor del parámetro event es "spiral", lo que hace que se cumpla la condición del if correspondiente.

Como resultado, se ejecuta la llamada a setState(new SpiralState()), iniciando el cambio de estado de la partícula. Además, se puede observar que el puntero state comienza a apuntar a una instancia de SpiralState.

<img width="933" height="588" alt="image" src="https://github.com/user-attachments/assets/29065bfd-07d9-490f-81a1-6630330a6820" />
Se eligió este punto de inspección dentro del método setState porque es el momento en el que se realiza la asignación del nuevo estado a la partícula. Esto permite verificar directamente que el puntero state cambia y pasa a apuntar a una instancia de SpiralState.

De esta forma, se confirma que el evento recibido ha producido correctamente la transición de estado, completando el flujo desde la notificación hasta la modificación del comportamiento de la partícula.

EXPLICACIÓN:
Al presionar la tecla 'p', se ejecuta el método keyPressed, donde se llama a notify("spiral"). Este método envía el evento a todas las partículas registradas como observadores.

Luego, en el método onNotify, cada partícula recibe el evento "spiral", lo que provoca la ejecución de setState(new SpiralState()).

Finalmente, en el método setState, se observa que el puntero state es actualizado con el valor de newState, y ambos apuntan a una instancia de SpiralState, confirmando el cambio de estado.

JUSTIFICACIÓN:
Los puntos de inspección elegidos permiten seguir paso a paso cómo un evento generado por el usuario se propaga mediante el patrón Observer y desencadena un cambio de estado en las partículas.

Esto demuestra la correcta integración entre los patrones Observer y State, permitiendo modificar dinámicamente el comportamiento de los objetos en tiempo de ejecución.

EVIDENCIA 4:

<img width="925" height="504" alt="image" src="https://github.com/user-attachments/assets/ccaaabb3-f4c7-4578-a538-bc5b433ddc03" />
En la captura se observa que el puntero state apunta a una instancia de SpiralState. Además, en su _vtable se encuentra la función SpiralState::update, lo que indica que el comportamiento ejecutado corresponde específicamente a esta clase.

EXPLICACIÓN:
El estado SpiralState fue implementado como una clase que hereda directamente de State, en lugar de reutilizar o extender otro estado existente como NormalState o AttractState.

La presencia de SpiralState::update en la _vtable demuestra que este estado tiene su propia implementación del método virtual update, independiente de otros estados como NormalState o AttractState.

JUSTIFICACIÓN:
Se decidió heredar directamente de State para mantener una separación clara de responsabilidades y evitar dependencias innecesarias entre estados. Cada estado representa un comportamiento distinto, por lo que no es adecuado reutilizar lógica de otro estado que responde a una intención diferente.

Esta decisión mejora la claridad del diseño, facilita el mantenimiento del código y permite agregar nuevos estados sin afectar los existentes, aprovechando correctamente el polimorfismo.




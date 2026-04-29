# Unidad 5
## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 

ofApp.h

``` c++
#pragma once
#include "ofMain.h"
#include <vector>

class Particle {
public:
	virtual ~Particle() { }
	virtual void update(float dt) = 0;
	virtual void draw() = 0;
	virtual bool isDead() const = 0;
	virtual bool shouldExplode() const { return false; }
	virtual glm::vec2 getPosition() const { return glm::vec2(0, 0); }
	virtual ofColor getColor() const { return ofColor(255); }
};

class RisingParticle : public Particle {
protected:
	glm::vec2 position;
	glm::vec2 velocity;
	ofColor color;
	float lifetime;
	float age;
	bool exploded;

public:
	RisingParticle(const glm::vec2 & pos, const glm::vec2 & vel,
		const ofColor & col, float life)
		: position(pos)
		, velocity(vel)
		, color(col)
		, lifetime(life)
		, age(0)
		, exploded(false) { }

	void update(float dt) override {
		position += velocity * dt;
		age += dt;
		velocity.y += 9.8f * dt * 8;
		float explosionThreshold = ofGetHeight() * 0.15f + ofRandom(-30, 30);
		if (position.y <= explosionThreshold || age >= lifetime) {
			exploded = true;
		}
	}

	void draw() override {
		ofSetColor(color);
		ofDrawCircle(position, 10);
	}

	bool isDead() const override { return exploded; }
	bool shouldExplode() const override { return exploded; }
	glm::vec2 getPosition() const override { return position; }
	ofColor getColor() const override { return color; }
};

class ZigZagParticle : public Particle {
protected:
	glm::vec2 position;
	glm::vec2 velocity;
	ofColor color;
	float age;
	float lifetime;
	bool exploded;

public:
	ZigZagParticle(const glm::vec2 & pos)
		: position(pos)
		, velocity(ofRandom(-100, 100), -250)
		, color(ofColor::cyan)
		, age(0)
		, lifetime(2.5f)
		, exploded(false) { }

	void update(float dt) override {
		age += dt;
		position.x += sin(age * 10) * 100 * dt;
		position += velocity * dt;
		if (age >= lifetime) {
			exploded = true;
		}
	}

	void draw() override {
		ofSetColor(color);
		ofDrawCircle(position, 8);
	}

	bool isDead() const override { return exploded; }
	bool shouldExplode() const override { return exploded; }
	glm::vec2 getPosition() const override { return position; }
	ofColor getColor() const override { return color; }
};

class SpiralParticle : public Particle {
protected:
	glm::vec2 center;
	float angle;
	float radius;
	float speed;
	float age;
	float lifetime;
	bool exploded;
	ofColor color;

public:
	SpiralParticle(const glm::vec2 & pos)
		: center(pos)
		, angle(0)
		, radius(0)
		, speed(5)
		, age(0)
		, lifetime(2.5f)
		, exploded(false)
		, color(ofColor::magenta) { }

	void update(float dt) override {
		age += dt;
		angle += speed * dt;
		radius += 50 * dt;
		if (age >= lifetime) {
			exploded = true;
		}
	}

	void draw() override {
		glm::vec2 pos = center + glm::vec2(cos(angle), sin(angle)) * radius;
		ofSetColor(color);
		ofDrawCircle(pos, 6);
	}

	bool isDead() const override { return exploded; }
	bool shouldExplode() const override { return exploded; }
	glm::vec2 getPosition() const override {
		return center + glm::vec2(cos(angle), sin(angle)) * radius;
	}
	ofColor getColor() const override { return color; }
};

class ExplosionParticle : public Particle {
protected:
	glm::vec2 position;
	glm::vec2 velocity;
	ofColor color;
	float age;
	float lifetime;
	float size;

public:
	ExplosionParticle(const glm::vec2 & pos, const glm::vec2 & vel,
		const ofColor & col, float life, float sz)
		: position(pos)
		, velocity(vel)
		, color(col)
		, age(0)
		, lifetime(life)
		, size(sz) { }

	void update(float dt) override {
		position += velocity * dt;
		age += dt;
		float alpha = ofMap(age, 0, lifetime, 255, 0, true);
		color.a = alpha;
	}

	bool isDead() const override { return age >= lifetime; }
};

class CircularExplosion : public ExplosionParticle {
public:
	CircularExplosion(const glm::vec2 & pos, const ofColor & col)
		: ExplosionParticle(pos, glm::vec2(0, 0), col, 1.2f, ofRandom(16, 32)) {
		float angle = ofRandom(0, TWO_PI);
		float speed = ofRandom(80, 200);
		velocity = glm::vec2(cos(angle), sin(angle)) * speed;
	}

	void draw() override {
		ofSetColor(color);
		ofDrawCircle(position, size);
	}
};

class RandomExplosion : public ExplosionParticle {
public:
	RandomExplosion(const glm::vec2 & pos, const ofColor & col)
		: ExplosionParticle(pos, glm::vec2(0, 0), col, 1.5f, ofRandom(16, 32)) {
		velocity = glm::vec2(ofRandom(-200, 200), ofRandom(-200, 200));
	}

	void draw() override {
		ofSetColor(color);
		ofDrawRectangle(position.x, position.y, size, size);
	}
};

class StarExplosion : public ExplosionParticle {
public:
	StarExplosion(const glm::vec2 & pos, const ofColor & col)
		: ExplosionParticle(pos, glm::vec2(0, 0), col, 1.3f, ofRandom(20, 40)) {
		float angle = ofRandom(0, TWO_PI);
		float speed = ofRandom(90, 180);
		velocity = glm::vec2(cos(angle), sin(angle)) * speed;
	}

	void draw() override {
		ofSetColor(color);
		int rays = 5;
		float outerRadius = size;
		float innerRadius = size * 0.5f;
		ofPushMatrix();
		ofTranslate(position);
		for (int i = 0; i < rays; i++) {
			float theta = ofMap(i, 0, rays, 0, TWO_PI);
			float xOuter = cos(theta) * outerRadius;
			float yOuter = sin(theta) * outerRadius;
			float xInner = cos(theta + PI / rays) * innerRadius;
			float yInner = sin(theta + PI / rays) * innerRadius;
			ofDrawLine(0, 0, xOuter, yOuter);
			ofDrawLine(xOuter, yOuter, xInner, yInner);
		}
		ofPopMatrix();
	}
};

class TriangleExplosion : public ExplosionParticle {
private:
	float rotation; 
	float rotationSpeed; 

public:
	TriangleExplosion(const glm::vec2 & pos, const ofColor & col)
		: ExplosionParticle(pos, glm::vec2(0, 0), col, 1.4f, ofRandom(15, 30))
		, rotation(ofRandom(360))
		, rotationSpeed(ofRandom(120, 240)) {
		float angle = ofRandom(0, TWO_PI);
		float speed = ofRandom(100, 220);
		velocity = glm::vec2(cos(angle), sin(angle)) * speed;
	}

	void update(float dt) override {
		ExplosionParticle::update(dt); 
		rotation += rotationSpeed * dt;
	}

	void draw() override {
		ofPushMatrix();
		ofTranslate(position);
		ofRotateDeg(rotation);
		ofSetColor(color);
		float s = size;
		ofDrawTriangle(
			glm::vec2(0, -s),
			glm::vec2(-s * 0.8f, s),
			glm::vec2(s * 0.8f, s));
		ofPopMatrix();
	}
};

class ofApp : public ofBaseApp {
public:
	void setup();
	void update();
	void draw();
	void mousePressed(int x, int y, int button);
	void keyPressed(int key);

	std::vector<Particle *> particles;
	~ofApp();

private:
	void createRisingParticle();
	void createZigZagParticle();
	void createSpiralParticle();
};

```

ofApp.cpp

``` c++
#include "ofApp.h"

void ofApp::setup() {
	ofSetFrameRate(60);
	ofBackground(0);
}

void ofApp::update() {
	float dt = ofGetLastFrameTime();

	for (int i = 0; i < particles.size(); i++) {
		particles[i]->update(dt);
	}

	for (int i = particles.size() - 1; i >= 0; i--) {
		if (particles[i]->shouldExplode()) {
			int explosionType = (int)ofRandom(4);
			int numParticles = (int)ofRandom(20, 30);
			glm::vec2 explodePos = particles[i]->getPosition();
			ofColor explodeCol = particles[i]->getColor();

			for (int j = 0; j < numParticles; j++) {
				if (explosionType == 0) {
					particles.push_back(new CircularExplosion(explodePos, explodeCol));
				} else if (explosionType == 1) {
					particles.push_back(new RandomExplosion(explodePos, explodeCol));
				} else if (explosionType == 2) {
					particles.push_back(new StarExplosion(explodePos, explodeCol));
				} else {
					particles.push_back(new TriangleExplosion(explodePos, explodeCol));
				}
			}

			ofLog() << "[EXPLODE] Eliminando particula en posicion "
					<< explodePos << " | particles.size antes: " << particles.size();

			delete particles[i];
			particles[i] = nullptr;
			particles.erase(particles.begin() + i);

			ofLog() << "[EXPLODE] particles.size despues: " << particles.size();

		} else if (particles[i]->isDead()) {

			ofLog() << "[DEAD] Eliminando particula muerta | particles.size antes: "
					<< particles.size();

			delete particles[i];
			particles[i] = nullptr;
			particles.erase(particles.begin() + i);

			ofLog() << "[DEAD] particles.size despues: " << particles.size();
		}
	}
}

void ofApp::draw() {
	for (int i = 0; i < particles.size(); i++) {
		particles[i]->draw();
	}
}

void ofApp::createRisingParticle() {
	float minX = ofGetWidth() * 0.35f;
	float maxX = ofGetWidth() * 0.65f;
	float spawnX = ofRandom(minX, maxX);
	glm::vec2 pos(spawnX, ofGetHeight());
	glm::vec2 target(ofGetWidth() / 2.0f + ofRandom(-300, 300),
		ofGetHeight() * 0.10f + ofRandom(-30, 30));
	glm::vec2 direction = glm::normalize(target - pos);
	glm::vec2 vel = direction * ofRandom(250, 350);
	ofColor col;
	col.setHsb(ofRandom(255), 220, 255);
	float lifetime = ofRandom(1.5f, 3.5f);

	particles.push_back(new RisingParticle(pos, vel, col, lifetime));
}

void ofApp::createZigZagParticle() {
	glm::vec2 pos(ofRandomWidth(), ofGetHeight());
	particles.push_back(new ZigZagParticle(pos));
}

void ofApp::createSpiralParticle() {
	glm::vec2 pos(ofRandomWidth(), ofRandomHeight());
	particles.push_back(new SpiralParticle(pos));
}

void ofApp::mousePressed(int x, int y, int button) {
	int type = (int)ofRandom(3);
	if (type == 0)
		createRisingParticle();
	else if (type == 1)
		createZigZagParticle();
	else
		createSpiralParticle();
}

void ofApp::keyPressed(int key) {
	if (key == ' ') {
		ofLog() << "[LIMITE] Creando 500 particulas. Size antes: " << particles.size();
		for (int i = 0; i < 500; i++) {
			createRisingParticle();
		}
		ofLog() << "[LIMITE] Size despues: " << particles.size();
	}

	if (key == 'c' || key == 'C') {
		ofLog() << "[CLEAR] Vaciando vector. Size antes: " << particles.size();
		for (int i = 0; i < particles.size(); i++) {
			delete particles[i];
			particles[i] = nullptr;
		}
		particles.clear();
		ofLog() << "[CLEAR] Vector vaciado. Size: " << particles.size();
	}

	if (key == 's') {
		ofSaveScreen("screenshot_" + ofToString(ofGetFrameNum()) + ".png");
	}
}

ofApp::~ofApp() {
	for (int i = 0; i < particles.size(); i++) {
		delete particles[i];
	}
	particles.clear();
}


```

**EVIDENCIAS**

Evidencia 1:
<img width="951" height="553" alt="image" src="https://github.com/user-attachments/assets/87917482-d134-4741-a57f-a8003214b5f2" />
Elegí este punto porque aquí las partículas ya están dentro del vector y se están usando como Particle*, lo que permite ver cómo funciona la herencia en un objeto real durante la ejecución.

Explicación:
En la captura se observa que particles[i] es un puntero a Particle, pero el objeto real es de tipo RisingParticle.

Al expandir el objeto en el depurador se pueden ver sus atributos, como:

- position
- velocity
- color
- lifetime
- age
- exploded

Estos pertenecen a la clase RisingParticle, que es una clase hija de Particle.

También se observa la parte correspondiente a la clase base Particle, lo que indica que el objeto contiene información tanto de la clase padre como de la hija.

Justificación:
Esta captura demuestra cómo funciona la herencia en memoria.

Se puede ver que un objeto de la clase RisingParticle incluye dentro de sí la estructura de la clase base Particle. Es decir, el objeto no solo tiene sus propios datos, sino también lo que hereda de su clase padre.

Evidencia 2:
<img width="937" height="657" alt="image" src="https://github.com/user-attachments/assets/e3227a45-fde5-4f0b-8f34-9ccf75e2dcd6" />
<img width="935" height="637" alt="image" src="https://github.com/user-attachments/assets/4290401e-edc5-464b-ac73-aee342fe65f4" />
Elegí este punto porque en este momento las partículas se están usando como Particle*, lo que permite inspeccionar el puntero a la vtable (__vfptr) y ver cómo se resuelven las funciones virtuales en tiempo de ejecución.

Explicación:
En la captura se observa el campo __vfptr, que es el puntero a la tabla de funciones virtuales (vtable).

El objeto inspeccionado es de tipo TriangleExplosion, que corresponde a un nuevo tipo de partícula implementado. Al expandir la vtable, se observan funciones como:

TriangleExplosion::update(float)
TriangleExplosion::draw()
ExplosionParticle::isDead()
Particle::shouldExplode()
Particle::getPosition()
Particle::getColor()

Esto muestra que algunas funciones fueron redefinidas en el nuevo tipo (TriangleExplosion), mientras que otras se heredan de las clases base.

Justificación:
Esta evidencia demuestra cómo se implementa el polimorfismo en C++ usando la vtable.

El nuevo tipo TriangleExplosion tiene su propia tabla de funciones virtuales, lo que permite que el programa ejecute sus propias versiones de métodos como update() y draw(), incluso cuando el objeto es tratado como Particle*.

Esto confirma que cada subclase puede tener su propio comportamiento, y que la vtable es el mecanismo que permite esa diferencia en tiempo de ejecución.

Evidencia 3:

<img width="937" height="747" alt="image" src="https://github.com/user-attachments/assets/8a2697a3-e798-47af-8b05-713f0dcdde1a" />
Elegí este punto porque permite observar directamente qué función se está ejecutando en tiempo de ejecución para este tipo de partícula.

Explicación:
En la captura se observa que el programa se detiene dentro del método update(float dt) de la clase TriangleExplosion.

También se puede ver que la variable this es de tipo TriangleExplosion*, lo que indica que el objeto actual pertenece a esa clase.

Justificación:
Esta evidencia demuestra el polimorfismo en tiempo de ejecución.

Aunque las partículas se manejan como Particle*, el programa ejecuta la función correspondiente al tipo real del objeto, en este caso TriangleExplosion.

El hecho de que se esté ejecutando el método update() de esta clase confirma que el despacho dinámico funciona correctamente y que cada tipo de partícula puede tener su propio comportamiento.

Evidencia 4:

<img width="937" height="747" alt="image" src="https://github.com/user-attachments/assets/8a2697a3-e798-47af-8b05-713f0dcdde1a" />
Elegí este punto porque permite inspeccionar el objeto completo (this) y observar qué atributos están disponibles desde la subclase y cuáles provienen de las clases base.

Explicación:
En la captura se observa el objeto this, que es de tipo TriangleExplosion.

Al expandirlo, se pueden ver diferentes atributos como:

- rotation y rotationSpeed, que pertenecen a TriangleExplosion
- position, velocity, color, age, lifetime y size, que pertenecen a la clase ExplosionParticle

Estos atributos están accesibles dentro de la subclase porque están definidos como protected.

No se observan atributos privados de otras clases, lo que indica que esos no son accesibles directamente desde la subclase.

Justificación:
Esta evidencia demuestra el encapsulamiento en el contexto de herencia.

Se puede ver que la subclase TriangleExplosion tiene acceso a ciertos atributos heredados (como position y velocity) porque están definidos como protected, lo que permite reutilizar y modificar comportamiento.

Al mismo tiempo, los atributos privados no son visibles ni accesibles desde la subclase, lo que mantiene el control sobre los datos y protege la integridad de la clase base.

Esto confirma que el encapsulamiento controla qué información se puede heredar y utilizar en las clases derivadas.

Evidencia 5:

<img width="915" height="686" alt="image" src="https://github.com/user-attachments/assets/e95a3121-0d04-4b91-af17-4595bb7a45f0" />
<img width="937" height="747" alt="image" src="https://github.com/user-attachments/assets/8a2697a3-e798-47af-8b05-713f0dcdde1a" />
<img width="925" height="636" alt="image" src="https://github.com/user-attachments/assets/81f872cc-22b9-4a4b-b176-17644aaae4ce" />
<img width="940" height="643" alt="image" src="https://github.com/user-attachments/assets/170a4d7f-ba42-44f7-8085-d60592c052ad" />

Elegí estos puntos porque representan las tres etapas principales del ciclo de vida de un objeto: creación, uso y destrucción.

Explicación:
En las capturas se puede observar el recorrido completo de la partícula TriangleExplosion.

Primero, en la creación, la partícula es agregada al vector particles, lo que indica que entra al sistema.

Luego, durante el update(), la partícula sigue activa y actualiza sus valores, como la rotación, lo que muestra que está en uso.

Finalmente, en la eliminación, se observa que el objeto aún está en el vector justo antes de ser eliminado con delete, y luego es removido con erase.

Justificación:
Estos puntos de inspección demuestran claramente el ciclo de vida completo del objeto.

Se puede comprobar que la partícula:

- Se crea dinámicamente y se almacena en el vector
- Se mantiene activa mientras se actualiza en cada frame
- Se elimina correctamente cuando ya no es necesaria

Esto confirma que el programa gestiona adecuadamente la memoria y el uso de los objetos, evitando errores y asegurando un flujo correcto desde la creación hasta la destrucción.

Evidencia 6:

<img width="925" height="636" alt="image" src="https://github.com/user-attachments/assets/81f872cc-22b9-4a4b-b176-17644aaae4ce" />
<img width="940" height="643" alt="image" src="https://github.com/user-attachments/assets/170a4d7f-ba42-44f7-8085-d60592c052ad" />

Elegí este punto porque es donde se libera la memoria del objeto y se elimina del vector, lo que permite verificar que no queden referencias innecesarias.

Explicación:
En la captura se observa que el programa se detiene justo antes de ejecutar delete particles[i].

En ese momento, el objeto aún existe en memoria y está dentro del vector particles.

Después de ejecutar delete, la memoria del objeto se libera. Luego, el puntero se establece en nullptr y el elemento se elimina del vector con erase().

Esto evita que queden punteros inválidos o referencias a memoria que ya no se usa.

Justificación:
Esta evidencia demuestra que no hay fugas de memoria en el programa.

Cada objeto que se crea dinámicamente con new es eliminado correctamente con delete, y además se elimina del vector, evitando que queden referencias colgantes.

El uso de nullptr después del delete también ayuda a prevenir errores al acceder a memoria ya liberada.

Esto confirma que la gestión de memoria es correcta y que el programa no acumula objetos innecesarios en memoria.

Evidencia 7:

<img width="951" height="397" alt="image" src="https://github.com/user-attachments/assets/bd0a889c-9c5f-40a5-901d-ca380bfea14f" />

Elegí este escenario porque permite evaluar el comportamiento del programa bajo una carga alta, lo cual es una condición límite relevante.

Explicación:
En la captura se observa que el vector particles contiene una gran cantidad de elementos después de ejecutar la creación masiva.

Esto indica que el programa es capaz de manejar múltiples objetos simultáneamente sin fallar.

Durante la ejecución, cada partícula sigue siendo actualizada y eventualmente eliminada según su ciclo de vida.

Justificación:
Esta prueba demuestra que el sistema funciona correctamente incluso en una condición límite donde se crean muchas partículas al mismo tiempo.

Se verifica que:

- El vector puede almacenar una gran cantidad de objetos
- El programa no se bloquea ni presenta errores
- Las partículas siguen su ciclo de vida normal (creación, actualización y eliminación)

Esto confirma que la implementación es robusta y maneja adecuadamente situaciones de alta carga.










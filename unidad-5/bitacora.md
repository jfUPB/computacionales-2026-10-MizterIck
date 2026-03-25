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
public:
	float rotation;

	TriangleExplosion(const glm::vec2 & pos, const ofColor & col)
		: ExplosionParticle(pos, glm::vec2(0, 0), col, 1.4f, ofRandom(15, 30))
		, rotation(ofRandom(360)) {

		float angle = ofRandom(0, TWO_PI);
		float speed = ofRandom(100, 220);
		velocity = glm::vec2(cos(angle), sin(angle)) * speed;
	}

	void update(float dt) override {
		ExplosionParticle::update(dt);
		rotation += 180 * dt;
	}

	void draw() override {
		ofPushMatrix();
		ofTranslate(position);
		ofRotateDeg(rotation);
		ofSetColor(color);

		float s = size;
		ofDrawTriangle(
			glm::vec2(0, -s),
			glm::vec2(-s * 0.8, s),
			glm::vec2(s * 0.8, s));

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

			for (int j = 0; j < numParticles; j++) {
				if (explosionType == 0) {
					particles.push_back(new CircularExplosion(
						particles[i]->getPosition(), particles[i]->getColor()));
				} else if (explosionType == 1) {
					particles.push_back(new RandomExplosion(
						particles[i]->getPosition(), particles[i]->getColor()));
				} else if (explosionType == 2) {
					particles.push_back(new StarExplosion(
						particles[i]->getPosition(), particles[i]->getColor()));
				} else {
					particles.push_back(new TriangleExplosion(
						particles[i]->getPosition(), particles[i]->getColor()));
				}
			}

			delete particles[i];
			particles.erase(particles.begin() + i);
		} else if (particles[i]->isDead()) {
			delete particles[i];
			particles.erase(particles.begin() + i);
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
		for (int i = 0; i < 500; i++) {
			createRisingParticle();
		}
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

Evidencia 1

## Bitácora de reflexión

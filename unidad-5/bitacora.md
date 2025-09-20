# Evidencias de la unidad 5

### Actividad 2 

#### Act 2 1.1

__Pregunta__: ¿Qué concepto de otra unidad utilice?

__Respuesta__: De la unidad 1 utilice el random() donde lo que quise cambiar fue el origen del sistema de particulas, basicamente creando una variante origen que encuentra sus valores de x/y con random

__Pregunta__: ¿Cómo se maneja la desaparición de las particulas?

__Respuesta__: el sistema de particulas es un array que tiene todas las particulas generadas enlistadas, se hace un contador que en vez de sumar va restando para ir buscando las particulas que ya hayan cumplido el tiempo estimado para que termine el ciclo y por ende deba desaparecer

[Enlace a p5.js](https://editor.p5js.org/gokuru12/sketches/AvixdWQFr)

<details>
<summary> Aquí esta el código</summary>

  ```javascript
  // The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let particles = [];
let origin;        // posición de nacimiento de partículas
let lastChange = 0; // último cambio de origen

function setup() {
  createCanvas(640, 240);
  origin = createVector(width/2, 20); // punto inicial
}

function draw() {
  background(255);

  // cada 4 segundos cambiamos el origen
  if (millis() - lastChange > 4000) {
    origin = createVector(random(width), random(height/2)); 
    lastChange = millis();
  }

  // agregar una nueva partícula desde el origen actual
  particles.push(new Particle(origin.x, origin.y));

  // loop al revés para actualizar y borrar partículas muertas
  for (let i = particles.length - 1; i >= 0; i--) {
    let particle = particles[i];
    particle.run();
    if (particle.isDead()) {
      particles.splice(i, 1);
    }
  }
}

// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return (this.lifespan < 0.0);
  }
}

```
</details>

<img width="813" height="310" alt="image" src="https://github.com/user-attachments/assets/44811849-7425-4cf4-8b82-9ca11db89f16" />


#### Act 2 1.2

__Pregunta__: ¿Qué concepto de otra unidad utilice?

__Respuesta__: De la unidad 1 utilice la distribución no uniforme para darle una posibilidad no igual de que al hacer click salgan cuadrados, esto fue utilizando el metodo que enseñaron en la unidad de agregar más particulas con this.particle(new particle) pero ahora generando cuadrados y cambiando que al hacer click tiene una posibilidad de 1/4 de que salgan cuadrados en vez de circulos.

[Enlace a p5.js](https://editor.p5js.org/gokuru12/sketches/6y36N4XvS)

<details>

  <summary> Aquí esta el código </summary>
  
  ```javascript
// Clase base: círculo
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

// Variante: cuadrado
class SquareParticle extends Particle {
  constructor(x, y) {
    super(x, y);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(200, 50, 50, this.lifespan);
    square(this.position.x - 4, this.position.y - 4, 8); 
  }
}

class Emitter {
  constructor(x, y, type = "circle") {
    this.origin = createVector(x, y);
    this.particles = [];
    this.type = type; // "circle" o "square"
  }

  addParticle() {
    if (this.type === "circle") {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new SquareParticle(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].run();
      if (this.particles[i].isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

let emitters = [];

function setup() {
  createCanvas(640, 240);
  createP("click para agregar sistemas de partículas (25% cuadrados)");
}

function draw() {
  background(255);
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticle();
  }
}

function mousePressed() {
  // 25% probabilidad de cuadrados
  let type = random() < 0.25 ? "square" : "circle";
  emitters.push(new Emitter(mouseX, mouseY, type));
}

```
</details>

<img width="799" height="303" alt="image" src="https://github.com/user-attachments/assets/56bdeb39-1d34-465f-9a1b-49910ae45f84" />

#### Act 2 1.3

__Pregunta__: ¿Qué concepto de otra unidad utilice?

__Respuesta__: Utilice de la unidad 3 la fuerza del viento, principalmente porque quería hacer que se viera distinto la fuerza del viento, así se vería más el hecho de que las esferas solo caen como si fueran pelotas, mientras que el "confetti" como es papel el simple viento se lo lleva

[Enlace a p5.js](https://editor.p5js.org/gokuru12/sketches/7-7q35jXB)

<details>

<summary> Aquí esta el código</summary>

```javascript

class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    let r = random(1);
    if (r < 0.5) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}


```

  
</details>

### Apply 

__Pregunta__: ¿Cuál es el concepto de tu obra? ¿Qué quieres comunicar con ella?

__Respuesta__: Quería hacer una situación de viento y ambiente en distintos lugares, algo sencillo pero lindo que te hiciera sentir como es la experiencia del viento en distintos lugares

__Pregunta__: ¿Tiene polimorfismo?

__Respuesta__: Así es, todas las particulas son hijas de una variable de particula que define sus tiempos de vida

__Preguntas__: Conceptos usados de otras unidades

__Respuesta__: Unidad 1 utilice random para la aparición de las particulas. Unidad 2 un lerpcolor que para el polvo. Unidad 3 Fuerza del viento y gravedad. Unidad 4 ondas en las gotas

__Pregunta__: como gestionar el tiempo en memoria

__Respuesta__: Con el contador inverso, tienen un tiempo de vida de 3 segundos

[Enlace a p5.js](https://editor.p5js.org/gokuru12/sketches/G0APTts2G)

<details>

<summary> Aquí esta el código </summary>


```javascript

let environments;
let currentEnv;

let particles = [];
let windStrength = 0;

function setup() {
  createCanvas(600, 400);

  // Definimos los entornos y colores
  environments = {
    forest: {
      name: "Bosque",
      bg: color(144, 238, 144),
      base: color(34, 139, 34)
    },
    desert: {
      name: "Desierto",
      bg: color(255, 228, 181),
      base: color(210, 180, 140)
    },
    ocean: {
      name: "Mar",
      bg: color(0, 105, 148),
      base: color(25, 25, 112)
    }
  };

  currentEnv = environments.forest; // Comenzamos en bosque
}

function draw() {
  background(currentEnv.bg);

  // viento depende del mouse
  windStrength = map(mouseY, height, 0, 0, 1);
  windStrength = constrain(windStrength, 0, 1);

  // agregar partículas cada pocos frames
  if (frameCount % 5 === 0) {
    particles.push(new Dust());

    // 🌟 más waves y en cualquier lugar de la pantalla
    particles.push(new Wave(random(width), random(height)));
    particles.push(new Wave(random(width), random(height)));

    if (windStrength > 0.75) {
      particles.push(new Raindrop());
    }
  }

  // ejecutar partículas
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.run(currentEnv);
    if (p.isDead()) {
      particles.splice(i, 1);
    }
  }

  // texto con info
  noStroke();
  fill(0);
  textSize(14);
  text(`Lugar: ${currentEnv.name}`, 10, 20);
  text(`Viento: ${(windStrength * 100).toFixed(0)}%`, 10, 40);
}

function keyPressed() {
  if (key === "1") currentEnv = environments.forest;
  if (key === "2") currentEnv = environments.desert;
  if (key === "3") currentEnv = environments.ocean;
}

class Wave extends Particle {
  constructor(x, y) {
    super(x, y);
    this.size = random(20, 40);
    this.speed = random(0.02, 0.05);
    this.offset = random(TWO_PI);
    this.alpha = 255; // 🌟 transparencia
  }

  run(env) {
    // afectadas mucho por el viento
    let wind = createVector(windStrength * 0.5, 0);
    this.applyForce(wind);

    this.update();
    this.show(env);

    // 🌟 desvanecimiento gradual
    this.alpha -= 2;
    this.lifespan = this.alpha;
  }

  show(env) {
    noFill();
    stroke(255, this.alpha); // blanco con transparencia
    strokeWeight(2);

    let waveY = this.position.y + sin(frameCount * this.speed + this.offset) * 10;
    ellipse(this.position.x, waveY, this.size, this.size / 2);
  }
}

class Raindrop extends Particle {
  constructor() {
    super(random(width), 0); // Aparece desde arriba
    this.size = 6;
  }

  run(env) {
    let gravity = createVector(0, 0.2);
    this.applyForce(gravity);

    // viento también las afecta
    let wind = createVector(windStrength * 0.3, 0);
    this.applyForce(wind);

    this.update();
    this.show(env);

    // cuando toca el suelo, genera onda
    if (this.position.y >= height - 2) {
      this.splash();
      this.lifespan = 0; // muere tras el impacto
    }
  }

  show(env) {
    noStroke();
    fill(0, 0, 255, 180);
    ellipse(this.position.x, this.position.y, this.size, this.size * 2);
  }

  splash() {
    noFill();
    stroke(0, 0, 255, 150);
    // 🌟 onda más grande que antes
    ellipse(this.position.x, height - 2, 20, 8);
  }
}

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.lifespan = 180; // 3s a 60 fps
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan--;
  }

  isDead() {
    return this.lifespan <= 0;
  }
}

class Dust extends Particle {
  constructor() {
    super(random(width), 0); // Aparece en el techo (y=0)
    this.size = 4;
  }

  run(env) {
    // viento suave (menos afectado que otras partículas)
    let wind = createVector(windStrength * 0.2, 0);
    this.applyForce(wind);

    // 🌟 gravedad añadida para que caiga
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);

    this.update();
    this.show(env);
  }

  show(env) {
    let darker = lerpColor(env.base, color(0), 0.5);
    let c = lerpColor(darker, env.base, random(0.3, 0.7));
    noStroke();
    fill(c);
    square(this.position.x, this.position.y, this.size);
  }
}



```

  
</details>

<img width="753" height="514" alt="image" src="https://github.com/user-attachments/assets/ee4882c5-e99b-439b-985c-b401df5e4051" />

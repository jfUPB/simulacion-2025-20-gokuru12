# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 5

_Pregunta_: se plantea un codigo para hacer calculo de las fuerzas de aceleración, hay 3 preguntas ¿Qué problema le ves a este planteamiento?
¿Qué solución propones?
¿Cómo lo implementarías en p5.js?

_Respuesta_: 
¿Que problema hay?: La aceleración se esta sumando en cada instancia una y otra vez, aunque la aceleración debe sumarse con la velocidad anterior, no es la suma de todas las anteriores solo la exacta anterior, esto pasa porque se esta sumando justo en el vector 
¿Solución que proponga? Tener un metodo que utilice el valor de la aceleración, sumar la velocidad actual y luego borrar la aceleración anterior para suplantarla con la nueva, haciendo esto en ciclo.
¿Como lo implementaría en p5.js? update() {
    this.velocity.add(this.acceleration);  
    this.position.add(this.velocity);     
    this.acceleration.mult(0);      
  }

### Actividad 6

_Pregunta_: ¿Por qué es necesario multiplicar la aceleración por cero en cada frame?
¿Por qué se multiplica por cero justo al final de update()?

_Respuesta_: Se multiplica por cero para que en la siguiente sumatoria que se vaya a hacer no vaya a acumular todos los anteriores. Y se multiplica al final porque así se asegura de que se pueda mover con la aceleración y luego de que el frame completo pase y este por terminar solo ahí es que elimina el dato cuando no lo necesitas más

### Actividad 7

_Pregunta_: ¿Que es lo raro o cual es el error en el ejemplo que nos muestran? 

_Respuesta_: Estas dividiendo directamente la fuerza, al hacer eso si quieres calcular en otro momento o en otro lugar usando esa misma fuerza ya no será la misma porque la cambiaste, lo mejor sería hacer una copia y modificar como afecta la copia al objeto.

### Actividad 8

_Pregunta_: ¿Cuál es la diferencia entre las dos líneas?
¿Qué podría salir mal con let friction = this.velocity;
En el fragmento de código ¿Cuándo es por VALOR y cuándo por REFERENCIA.

_Respuesta_: La diferencia es que uno modifica la copia (la que tiene el copy) y el otro modifica directamente la referencia
Que podría salir mal con let friction = this.velocity, que apenas se modifique un valor, la velocidad ya no será la misma así que cambiaría toda la referencia para todos los momentos y todos los otros calculos
El valor es el this.velocity.copy(); ya que este solo usa el valor que tenga la referencia pero sin modificar la referencia, mientras que el otro es la referencia en si

### Actividad 9

#### Fricción
_¿Como modelé la fricción?_: copie el valor de la velocidad y lo multiplique por -1 para hacerlo un vector contrario usando un porcentaje de 10% de la velocidad para que vaya desacelerando la pelota mientras esta tocando el suelo.

_¿Como se relaciona la fuerza con la obra?_: La verdad es un ejemplo sencillo de una pelota rebotando, quería asegurarme de la funcionalidad, cuando la pelota no toca el suelo no se detiene, cuando la toca va parando, es decisión del usuario si se detiene o si va saltando

Enlace: https://editor.p5js.org/gokuru12/sketches/zir9fAAVw

Código: let mover;

function setup() {
  createCanvas(600, 400);
  mover = new Mover(width/2, height-20, 20); // empieza en el suelo
  mover.velocity.x = 5; // impulso inicial hacia la derecha
}

function draw() {
  background(255);

  // --- GRAVEDAD ---
  let gravity = createVector(0, 0.3); // gravedad suave
  mover.applyForce(gravity);

  // --- FRICCIÓN ---
  let c = 0.1; // coeficiente de fricción
  let friction = mover.velocity.copy();
  friction.mult(-1);         
  friction.setMag(c);        
  mover.applyForce(friction);

  // --- SALTO SOLO SI ESTÁ EN EL SUELO ---
  if (mouseIsPressed && mover.onGround()) {
    let jump = createVector(0, -20); 
    mover.applyForce(jump);
  }

  // --- ACTUALIZAR Y DIBUJAR ---
  mover.update();
  mover.bounceEdges();
  mover.show();
}


// ---------------- CLASE ----------------
class Mover {
  constructor(x, y, m) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.mass = m;
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    fill(175, 100, 200);
    ellipse(this.position.x, this.position.y, this.mass*2, this.mass*2);
  }

  bounceEdges() {
    if (this.position.x > width - this.mass) {
      this.position.x = width - this.mass;
      this.velocity.x *= -1;
    } else if (this.position.x < this.mass) {
      this.position.x = this.mass;
      this.velocity.x *= -1;
    }

    if (this.position.y > height - this.mass) {
      this.position.y = height - this.mass;
      this.velocity.y = 0; // se queda en el suelo
    }
  }

  onGround() {
    return (this.position.y >= height - this.mass - 0.1);
  }
}

Captura: <img width="725" height="504" alt="image" src="https://github.com/user-attachments/assets/3c9e7b94-a263-4eb6-8243-ec001671bc6f" />

#### Resistencia del aire y fluidos

Como lo modele: Mi objetivo era hacer algo parecido a un lerp, donde cuando calcula la posición del mouse intenta perseguirlo haciendo que el mouse con la distancia genere una fuerza, fuerza que se ve reducida por el vector de velocidad invertida pero con un porcentaje de frenado.

Conceptualmente como se ejemplifica: es como una estrella en el oceano, donde va flotando por todos lados intentando seguirte

Enlace: https://editor.p5js.org/gokuru12/sketches/1xCBaDyB4

Código: let estrella;
let velocidad;

let resistencia = 0.05; // coeficiente de fricción del agua

function setup() {
  createCanvas(600, 400);
  estrella = createVector(width/2, height/2);
  velocidad = createVector(0, 0);
}

function draw() {
  background(0, 0, 255, 30);

  // vector hacia el mouse
  let objetivo = createVector(mouseX, mouseY);
  let direccion = p5.Vector.sub(objetivo, estrella);

  // usar dirección como "fuerza"
  // mientras más lejos del mouse, más fuerte el empuje
  velocidad.add(direccion.mult(0.01)); // pequeña aceleración hacia el mouse

  // aplicar fricción de agua (proporcional a la velocidad)
  let friccion = velocidad.copy();
  friccion.mult(-resistencia); // siempre opuesta a la velocidad
  velocidad.add(friccion);

  // actualizar posición
  estrella.add(velocidad);

  // dibujar estrella
  noStroke();
  fill(255, 255, 0);
  star(estrella.x, estrella.y, 10, 20, 5);
}

function star(x, y, r1, r2, npoints) {
  let angle = TWO_PI / npoints;
  let halfAngle = angle / 2.0;
  beginShape();
  for (let a = 0; a < TWO_PI; a += angle) {
    let sx = x + cos(a) * r2;
    let sy = y + sin(a) * r2;
    vertex(sx, sy);
    sx = x + cos(a + halfAngle) * r1;
    sy = y + sin(a + halfAngle) * r1;
    vertex(sx, sy);
  }
  endShape(CLOSE);
}

Foto: <img width="737" height="492" alt="image" src="https://github.com/user-attachments/assets/509a5fd9-f9e1-4cba-b42f-734d980aacf8" />



# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Aplique la del pendulo, fue mi herramienta principal. Use lo que nos enseñaron en códgio de la actividad 10, pero quitandole la parte de "agarrar" el pendulo y usando el click para cambiar el punto central

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: Use el aplicar fuerzas como la fricción creando las esferas que relentizan el pendulo central, además haciendo que se muevan algo aleatorios para que sea todo menos predecible
>

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: No aplique ningún conocimiento de la unidad 2.... o al menos no de forma intencional 
>

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: Coloque que hay una posibilidad de que la esfera haga el pequeño "fly" donde en cualquier momento puede dar un salto y hacer que el patron cambie. Tambien aplique movimiento aleatorio a las esferas de agua
>

## ¿Cómo resolviste la interacción?
> Tu respuesta aquí: Principalmente preguntandome que quería hacer como obra y luego agregando poco a poco cada idea, la idea principal era un pendulo que creará una linea roja, luego fue pensar en que como quiero volver un poco más caotico y me acorde de la posiblidad de que haga saltos, por lo que el dibujo rojo quedaría distinto, posteriormente me intereso la idea de hacer algo que frenará el pendulo, por lo que hice que el halo tuviera bastante transparencia para que fuera más visible los lugares donde pase más tiempo.
>

## Enlace a la obra en el editor de p5.js

[Aquí está mi obra](https://editor.p5js.org/gokuru12/sketches/7wvsoBVNj)

## Código de la obra 

``` js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Pendulum con efectos especiales y esferas de agua

class Pendulum {
  constructor(x, y, r) {
    // Fill all variables
    this.pivot = createVector(x, y);
    this.bob = createVector();
    this.r = r;
    this.angle = PI / 4;

    this.angleVelocity = 0.0;
    this.angleAcceleration = 0.0;
    this.damping = 0.995;
    this.ballr = 24.0;
    
    // Nuevas variables para los efectos
    this.dragging = false;
    this.trail = [];
    this.forceApplied = false;
    this.teleportChance = 0.005;
    this.haloAlpha = 40; // 1. Variable para controlar transparencia del halo (0-255)
  }

  update(waterSpheres) {
    // 4. Posibilidad de teletransporte (0.3% por frame)
    if (random() < this.teleportChance && !this.dragging) {
      let futureAngle = this.angle + this.angleVelocity * 60;
      let futureBob = createVector(
        this.r * sin(futureAngle),
        this.r * cos(futureAngle)
      );
      futureBob.add(this.pivot);
      
      this.trail.push({
        pos: futureBob.copy(),
        intensity: 50,
        isTeleport: true,
        size: this.ballr * 0.8
      });
      
      this.angle = futureAngle;
      this.bob.set(this.r * sin(this.angle), this.r * cos(this.angle), 0);
      this.bob.add(this.pivot);
      
      return;
    }

    if (!this.dragging) {
      let gravity = 0.4;
      this.angleAcceleration = ((-1 * gravity) / this.r) * sin(this.angle);
      this.angleVelocity += this.angleAcceleration;
      this.angle += this.angleVelocity;
      
      // 2. Verificar colisión con esferas de agua
      let inWater = false;
      for (let sphere of waterSpheres) {
        let d = dist(this.bob.x, this.bob.y, sphere.pos.x, sphere.pos.y);
        if (d < this.ballr + sphere.size * 0.85) {
          inWater = true;
          break;
        }
      }
      
      // Aplicar más damping si está en agua
      let currentDamping = inWater ? this.damping * 0.8 : this.damping;
      this.angleVelocity *= currentDamping;
    }
    
    this.bob.set(this.r * sin(this.angle), this.r * cos(this.angle), 0);
    this.bob.add(this.pivot);
    
    this.trail.push({
      pos: this.bob.copy(),
      intensity: 30,
      isTeleport: false,
      size: this.ballr * 0.8
    });
  }

  show() {
    // 1. Usar haloAlpha variable para la transparencia
    for (let i = 0; i < this.trail.length; i++) {
      let point = this.trail[i];
      let alpha = map(point.intensity, 0, 100, this.haloAlpha * 0.1, this.haloAlpha);
      
      if (point.isTeleport) {
        fill(255, 0, 0, alpha);
        noStroke();
        ellipse(point.pos.x, point.pos.y, point.size * 2);
      } else {
        fill(255, 0, 0, alpha);
        noStroke();
        ellipse(point.pos.x, point.pos.y, point.size * 2);
      }
    }

    stroke(0);
    strokeWeight(2);
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);
    
    fill(127);
    if (this.dragging) {
      fill(200, 0, 0);
    }
    circle(this.bob.x, this.bob.y, this.ballr * 2);
    
    fill(0);
    noStroke();
    circle(this.pivot.x, this.pivot.y, 8);
  }

  applyForce(direction) {
    let force = direction * 0.05;
    this.angleVelocity += force;
    this.forceApplied = true;
    
    this.trail.push({
      pos: this.bob.copy(),
      intensity: 40,
      isTeleport: false,
      size: this.ballr * 0.8
    });
  }

  changePivot(x, y) {
    this.pivot.set(x, y);
  }

  clicked(mx, my) {
    let d = dist(mx, my, this.bob.x, this.bob.y);
    if (d < this.ballr) {
      this.dragging = true;
    }
  }

  stopDragging() {
    this.angleVelocity = 0;
    this.dragging = false;
  }

  drag() {
    if (this.dragging) {
      let diff = p5.Vector.sub(this.pivot, createVector(mouseX, mouseY));
      this.angle = atan2(-1 * diff.y, diff.x) - radians(90);
    }
  }
}

// 2. Clase para las esferas de agua azules
class WaterSphere {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(random(-1, 1), random(-1, 1));
    this.size = random(18, 22); // Tamaño similar pero más pequeño
    this.color = color(100, 150, 255, 150); // Azul claro con transparencia
  }
  
  update() {
    // Movimiento aleatorio suave
    this.pos.add(this.vel);
    
    // Cambiar dirección ocasionalmente
    if (random() < 0.02) {
      this.vel.rotate(random(-0.5, 0.5));
    }
    
    // Rebotes en los bordes
    if (this.pos.x < 0 || this.pos.x > width) this.vel.x *= -1;
    if (this.pos.y < 0 || this.pos.y > height) this.vel.y *= -1;
    
    // Mantener dentro del canvas
    this.pos.x = constrain(this.pos.x, 0, width);
    this.pos.y = constrain(this.pos.y, 0, height);
  }
  
  show() {
    fill(this.color);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.size * 2);
  }
}

let pendulum;
let waterSpheres = [];

function setup() {
  createCanvas(800, 600);
  pendulum = new Pendulum(width / 2, height / 2, 175);
  
  // 2. Crear 6 esferas de agua azules
  for (let i = 0; i < 6; i++) {
    waterSpheres.push(new WaterSphere());
  }
}

function draw() {
  background(240);
  
  // Actualizar esferas de agua primero
  for (let sphere of waterSpheres) {
    sphere.update();
    sphere.show();
  }
  
  // Actualizar y mostrar péndulo
  pendulum.update(waterSpheres);
  pendulum.show();
  
  // Mostrar instrucciones y controles
 
}

function mousePressed() {
  pendulum.changePivot(mouseX, mouseY);
  pendulum.stopDragging();
}

function mouseReleased() {
  pendulum.stopDragging();
}

function keyPressed() {
  if (keyCode === LEFT_ARROW) {
    pendulum.applyForce(-1);
  } else if (keyCode === RIGHT_ARROW) {
    pendulum.applyForce(1);
  }
  
}
```

## Captura de pantalla representativa

<img width="700" height="445" alt="image" src="https://github.com/user-attachments/assets/1b4f2656-567d-4552-9cb0-d545043aaff6" />








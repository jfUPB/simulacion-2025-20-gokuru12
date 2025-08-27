# Unidad 3

## 🛠 Fase: Apply

### Actividad 10

#### Explicación

Este diseño trata de hacerte sentir un creador y controlador, las 2 cosas que puedes hacer principalmente son crear más esferas tanto una sola con el click izquierdo como tres con el click derecho. Junto a esto cuando acercas el mouse a las esferas notarás que se colocan rojas y más grandes, esto es porque aumentas su masa y empiezan a tener mayor fuerza de atracción que el resto, tu decides quien sigue a quien y cuantos los siguen. 

Además en caso de querer desmantelar todo no necesitas volver a iniciar el programa, con el botón c puedes eliminar todas las esferas e iniciar desde 0 si lo deseas.

__Pregunta__: ¿Como modelaste el sistema n-cuerpos?

__Respuesta__: Hay varias partes que considerar para modelar los n-cuerpos, el primer paso fue entender que no es que los objetos se acerquen a otro, sino que la masa de otro es la que jala al objeto. Esta lógica se aplica en cada uno de las esferas, luego hay que calcular el vector para ver la dirección, la distancia para ver que tanta fuerza se aplica para calcular la aceleración usando la fuerza divido la masa.

__Pregunta__: Enlace de p5

[Enlace a p5.js](https://editor.p5js.org/gokuru12/sketches/JUt8eL-sQ)


__Pregunta__: Copia el código

<details> 

  
<summary> Aquí esta el código </summary>

```javascript
class Body {
  constructor(x, y, mass) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-1, 1));
    this.acceleration = createVector(0, 0);
    this.mass = mass;
    this.originalMass = mass; // Guardar masa original
    this.radius = mass * 8;
    this.baseColor = color(random(100, 200), random(100, 200), random(100, 200));
    this.trail = [];
    this.maxTrailLength = 15; // Reducido para mejor rendimiento
  }

  attract(other) {
    let force = p5.Vector.sub(this.position, other.position);
    let distance = constrain(force.mag(), 5, 25);
    
    // Aumentar fuerza de atracción si está cerca del mouse
    let attractionMultiplier = 1.0;
    let mouseDist = dist(mouseX, mouseY, this.position.x, this.position.y);
    if (mouseDist < 200) {
      // Multiplicador progresivo: más cerca = más fuerza
      attractionMultiplier = map(mouseDist, 0, 200, 2.5, 1.0);
    }
    
    let strength = (0.4 * this.mass * other.mass * attractionMultiplier) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    // Actualizar masa según proximidad al mouse
    let mouseDist = dist(mouseX, mouseY, this.position.x, this.position.y);
    if (mouseDist < 200) {
      // Aumentar masa progresivamente (hasta 1.5x la masa original)
      this.mass = this.originalMass * map(mouseDist, 0, 200, 1.5, 1.0);
    } else {
      this.mass = this.originalMass;
    }
    this.radius = this.mass * 8;
    
    // Guardar posición actual para la estela
    this.trail.push({
      position: this.position.copy(),
      radius: this.radius,
      color: this.getCurrentColor()
    });
    
    if (this.trail.length > this.maxTrailLength) {
      this.trail.shift();
    }
    
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  getCurrentColor() {
    let mouseDist = dist(mouseX, mouseY, this.position.x, this.position.y);
    let colorFactor = map(mouseDist, 0, 200, 0, 1);
    colorFactor = constrain(colorFactor, 0, 1);
    
    if (mouseDist < 200) {
      return lerpColor(color(255, 0, 0), this.baseColor, colorFactor);
    } else {
      return this.baseColor;
    }
  }

  show() {
    let currentColor = this.getCurrentColor();
    
    // Dibujar estela (mismo tamaño que la esfera)
    for (let i = 0; i < this.trail.length; i++) {
      let alpha = map(i, 0, this.trail.length, 80, 10);
      let trailColor = this.trail[i].color;
      trailColor.setAlpha(alpha);
      
      fill(trailColor);
      noStroke();
      ellipse(
        this.trail[i].position.x, 
        this.trail[i].position.y, 
        this.trail[i].radius * 2
      );
    }
    
    // Dibujar esfera principal
    stroke(0);
    strokeWeight(1);
    fill(currentColor);
    ellipse(this.position.x, this.position.y, this.radius * 2);
  }
}

let bodies = [];

function setup() {
  createCanvas(800, 400);
  
  // Generar entre 6 y 8 esferas iniciales aleatorias
  let initialBodies = floor(random(6, 9));
  for (let i = 0; i < initialBodies; i++) {
    bodies.push(new Body(
      random(width), 
      random(height), 
      random(0.5, 3)
    ));
  }
}

function draw() {
  background(255, 30);

  // Primero calcular todas las fuerzas
  for (let i = 0; i < bodies.length; i++) {
    for (let j = 0; j < bodies.length; j++) {
      if (i !== j) {
        let force = bodies[j].attract(bodies[i]);
        bodies[i].applyForce(force);
      }
    }
  }
  
  // Luego actualizar y mostrar
  for (let i = 0; i < bodies.length; i++) {
    bodies[i].update();
    bodies[i].show();
  }
  
  
}

function mousePressed() {
  if (mouseButton === LEFT) {
    bodies.push(new Body(mouseX, mouseY, random(0.5, 3)));
  } else if (mouseButton === RIGHT) {
    for (let i = 0; i < 3; i++) {
      bodies.push(new Body(
        random(width), 
        random(height), 
        random(0.5, 3)
      ));
    }
  }
}

function keyPressed() {
  if (key === 'c' || key === 'C') {
    bodies = [];
  }
}


```

</details>

__Pregunta__: Captura de la actividad

<img width="918" height="379" alt="image" src="https://github.com/user-attachments/assets/5bcac3de-f6a0-4f09-bfce-a1e8350607ef" />

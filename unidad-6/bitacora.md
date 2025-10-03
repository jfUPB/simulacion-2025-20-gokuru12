# Evidencias de la unidad 6

## Actividad 1 

__Pregunta__: Mostrar dos imagenes que te interesaron de Tyler hobbs y explica el porque

__Respuesta__: 

#### 1. 

Esta obra me dió demasiada curiosidad especialmente porque se nota que los colores tienen un tipo de reglamento, pero no es del todo fijo, aunque hay lugares donde se ve más agrupación de un color se puede ver que sigue habiendo lineas "perdidas" que siguen dando color en lugares donde aparenta que no debería colorear. Las reglas que definien esta obra me causan mucha curiosidad

<img width="730" height="544" alt="image" src="https://github.com/user-attachments/assets/ecd2380d-9e22-450b-aded-3c7f025ff11f" />


#### 2.

Esta otra me hace preguntarme como se crearon las semi-esferas que hay que "bloquean" el fluir, que crean una sensación como de agua fluyendo en un rio pero me parece más curioso porque me da la sensación de que las mismas semi-esferas siguen unas reglas y que el agua sigue otras, me hace creer que parecen cosas distintas pero no lo son en realidad

<img width="1511" height="868" alt="image" src="https://github.com/user-attachments/assets/7e9309b0-be08-4ff2-9fbd-e6ce35db5a20" />


## Actividad 2

__¿Qué es la fuerza de dirección(steering force)?__ : Es la fuerza del "deseo" que tiene un vehiculo, dependiendo de que tanto desee algo se moverá con mayor o menor fuerza hacia un objeto

__¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?__: Las fuerzas que simulan la naturaleza solo pasan y afectan al ambiente sin más, mientras que la steering force es más bien un tipo de "razocinio" que tienen los vehiculos, ellos deciden dependiendo de lo que deseen, teman, cuiden entre otras propiedades que uno les puede dar, no es algo que simplemente les hace efecto, sino que tienen sus criterios para situaciones propias

__¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?__: Casí todas sus obras tienen toda la experiencia visual del steering force, y funciona como comportamiento animal porque en las mentes de los animales suelen ser menos razonales y más empiricos, se dejan llevar más por sus deseos, miedos etc, por lo que el steering force sirve para ello.


## Actividad 3

__Como se almacena el campo de flujo?__: Es una clase que maneja un array interno que en dicho array guarda la información de la dirección de los vectores en el flujo

__Qué estructura de datos se usa?__: Usa un array 2D donde usando el ruido perlin guarda las posiciones y vectores de las direcciones

__¿Qué representa cada elemento de esa estructura?__: Cada elemento es un paso para conseguir el producto final que es el campo de flujo, los multiples array, los calculos físicos, los movimientos todo ello es parte de la estructura para crear dicho producto.

__¿Cómo se calcula inicialmente el vector en cada punto?__ Despues de generar el campo donde estará el field, se calcula los vectores en el array posterior donde colocando las posiciones del array "j" e "i" 

__Analiza el coportamiento del vehiculo__: Cuando un vehiculo esta encima de un vector en el field hace constamente una pregunta de "en que dirección esta viendo el vector en el que estoy parado" para así seguir con su desired la dirección y fuerza de dicho vector, ahora por otro lado el como afecta al desired es que como cualquier fuerza se calcula como lo afecta, principalmente lo que se hace es sumar o restar los vectores del steering con los del campo de flujo

## Actividad 4

__para cada una de las tres reglas, explica con tus propias palabras__: Las 3 reglas son, separación, alineación y cohesión, Separación es una fuerza que dice "si mi amigo esta muy muy cerca me tengo que alejar un poco por el espacio personal" la alineación es una donde los agentes se preguntan "a que dirección ven los demás" para intentar seguir un vector parecido y la cohesión es sobre intentar mantenerse todos como manada o grupo grande, haciendo que se mantengan unidos, aunque con un limite de distancia porque en cierto momento "dejan de verse"


## Actividad 5 (Apply)

### Diseño: 

El objetivo era hacer un tipo de ambiente que se fuera "calentando" con la canción, que los autónomos pudieran sentir esa música y se reflejará en ellos como se siente la canción, ya que la canción es algo cercana, movida en ocasiones, lenta en otras, el objetivo es que refleje esta forma musical en la experiencia que yo estoy dando, principalmente como una clase de DJ visual

Mi primeros entendimientos de la canción y que elementos voy a usar para "tocar" la música

<img width="932" height="521" alt="image" src="https://github.com/user-attachments/assets/536cf40a-2fc7-4d7a-b3f7-0707d8d4b580" />

Mi primer boceto de más o menos como quiero que sea

<img width="938" height="523" alt="image" src="https://github.com/user-attachments/assets/d07d902c-fa33-41ea-a44c-76fabc30d656" />

### Enlace

[Enlace a p5.js](https://editor.p5js.org/gokuru12/sketches/bi8RmoZVg)

### Código
<details>

<Summary> Aquí esta el código </Summary>

```javascript

// Variables globales
let agents = [];
let waves = [];
let bassWaves = [];
let field = [];
let fieldSize = 20;
let cols, rows;
let song;
let fft;
let bassThreshold = 0.85; // MODIFICADO: más alto para menos sensibilidad
let state = 1; // 1: Tranquilo, 2: Bueno, 3: Intenso
let speedMode = 2; // 1: Lento, 2: Medio, 3: Rápido
let trailEnabled = false;
let trails = [];
let colorFrom, colorTo;
let currentBgColor;
let timeOffset = 0;

// Configuración inicial
function setup() {
    createCanvas(600, 400); // MODIFICADO: canvas más pequeño
    cols = floor(width / fieldSize);
    rows = floor(height / fieldSize);
    
    // Inicializar campo de vectores
    generateField();
    
    // Crear agentes MODIFICADO: 120 en lugar de 100
    for (let i = 0; i < 120; i++) {
        agents.push(new Agent(random(width), random(height)));
    }
    
    // Configurar análisis de audio
    fft = new p5.FFT();
    
    // Colores iniciales
    setStateColors();
    currentBgColor = colorFrom;
    
    // Configurar botones de control
    document.getElementById('playButton').addEventListener('click', function() {
        if (song) song.play();
    });
    
    document.getElementById('pauseButton').addEventListener('click', function() {
        if (song) song.pause();
    });
    
    document.getElementById('audioFile').addEventListener('change', function(e) {
        if (e.target.files.length > 0) {
            if (song) song.stop();
            song = loadSound(URL.createObjectURL(e.target.files[0]));
        }
    });
}

// Dibujar en cada frame
function draw() {
    // Actualizar color de fondo con interpolación
    timeOffset += 0.005;
    let lerpAmount = map(sin(timeOffset), -1, 1, 0, 1);
    currentBgColor = lerpColor(colorFrom, colorTo, lerpAmount);
    background(currentBgColor);
    
    // Actualizar y mostrar rastros si están activados
    if (trailEnabled) {
        for (let i = trails.length - 1; i >= 0; i--) {
            trails[i].update();
            trails[i].display();
            if (trails[i].isDead()) {
                trails.splice(i, 1);
            }
        }
    }
    
    // Analizar audio y crear ondas de bajo si es necesario
    if (song && song.isPlaying()) {
        let spectrum = fft.analyze();
        let bassLevel = fft.getEnergy("bass") / 255;
        
        if (bassLevel > bassThreshold) {
            bassWaves.push(new BassWave(width/2, height/2));
        }
    }
    
    // Actualizar y mostrar ondas de bajo
    for (let i = bassWaves.length - 1; i >= 0; i--) {
        bassWaves[i].update();
        bassWaves[i].display();
        if (bassWaves[i].isDead()) {
            bassWaves.splice(i, 1);
        }
    }
    
    // Actualizar y mostrar ondas de miedo
    for (let i = waves.length - 1; i >= 0; i--) {
        waves[i].update();
        waves[i].display();
        if (waves[i].isDead()) {
            waves.splice(i, 1);
        }
    }
    
    // Actualizar y mostrar agentes
    for (let agent of agents) {
        // Aplicar comportamiento de flocking
        agent.flock(agents);
        
        // Aplicar miedo a las ondas
        agent.applyFear(waves);
        
        // Actualizar y mostrar agente
        agent.update();
        agent.display();
        
        // Añadir rastro si está activado
        if (trailEnabled) {
            trails.push(new Trail(agent.position.x, agent.position.y, agent.color));
        }
    }
}

// Generar campo de vectores
function generateField() {
    field = [];
    let noiseScale = 0.1;
    let time = millis() * 0.001;
    
    for (let i = 0; i < cols; i++) {
        field[i] = [];
        for (let j = 0; j < rows; j++) {
            let angle = noise(i * noiseScale, j * noiseScale, time) * TWO_PI;
            field[i][j] = p5.Vector.fromAngle(angle);
        }
    }
}

// Establecer colores según el estado
function setStateColors() {
    switch(state) {
        case 1: // Tranquilo - Azul a Morado
            colorFrom = color(65, 105, 225); // Azul real
            colorTo = color(147, 112, 219); // Morado medio
            break;
        case 2: // Bueno - Amarillo a Naranja
            colorFrom = color(255, 215, 0); // Oro
            colorTo = color(255, 140, 0);   // Naranja oscuro
            break;
        case 3: // Intenso - Rojo a Rosado
            colorFrom = color(220, 20, 60);  // Carmesí
            colorTo = color(255, 105, 180);  // Rosa fuerte
            break;
    }
}

// Clase Agente
class Agent {
    constructor(x, y) {
        this.position = createVector(x, y);
        this.velocity = p5.Vector.random2D();
        this.acceleration = createVector(0, 0);
        this.maxSpeed = this.getMaxSpeed();
        this.maxForce = 0.1;
        this.color = color(random(200, 255), random(200, 255), random(200, 255));
        this.size = 5;
        this.fear = 0;
    }
    
    getMaxSpeed() {
        switch(speedMode) {
            case 1: return 1;   // Lento
            case 2: return 2;   // Medio
            case 3: return 4;   // Rápido
            default: return 2;
        }
    }
    
    // Comportamiento de flocking
    flock(agents) {
        let separation = this.separate(agents);
        let alignment = this.align(agents);
        let cohesion = this.cohere(agents);
        
        // Ajustar pesos según el estado
        let sepWeight, aliWeight, cohWeight;
        
        switch(state) {
            case 1: // Tranquilo - menos deseo de estar juntos
                sepWeight = 1.5;
                aliWeight = 0.5;
                cohWeight = 0.5;
                break;
            case 2: // Bueno - más ganas de acercarse
                sepWeight = 1.0;
                aliWeight = 1.0;
                cohWeight = 1.5;
                break;
            case 3: // Intenso - les gusta estar juntos
                sepWeight = 0.5;
                aliWeight = 1.5;
                cohWeight = 2.0;
                break;
            default:
                sepWeight = 1.0;
                aliWeight = 1.0;
                cohWeight = 1.0;
        }
        
        separation.mult(sepWeight);
        alignment.mult(aliWeight);
        cohesion.mult(cohWeight);
        
        this.acceleration.add(separation);
        this.acceleration.add(alignment);
        this.acceleration.add(cohesion);
    }
    
    // Separación: evitar amontonamiento
    separate(agents) {
        let desiredSeparation = 25;
        let steer = createVector(0, 0);
        let count = 0;
        
        for (let other of agents) {
            let d = p5.Vector.dist(this.position, other.position);
            if (d > 0 && d < desiredSeparation) {
                let diff = p5.Vector.sub(this.position, other.position);
                diff.normalize();
                diff.div(d); // Peso por distancia
                steer.add(diff);
                count++;
            }
        }
        
        if (count > 0) {
            steer.div(count);
            steer.normalize();
            steer.mult(this.maxSpeed);
            steer.sub(this.velocity);
            steer.limit(this.maxForce);
        }
        return steer;
    }
    
    // Alineación: moverse en la misma dirección
    align(agents) {
        let neighborDist = 50;
        let sum = createVector(0, 0);
        let count = 0;
        
        for (let other of agents) {
            let d = p5.Vector.dist(this.position, other.position);
            if (d > 0 && d < neighborDist) {
                sum.add(other.velocity);
                count++;
            }
        }
        
        if (count > 0) {
            sum.div(count);
            sum.normalize();
            sum.mult(this.maxSpeed);
            let steer = p5.Vector.sub(sum, this.velocity);
            steer.limit(this.maxForce);
            return steer;
        }
        return createVector(0, 0);
    }
    
    // Cohesión: moverse hacia el centro del grupo
    cohere(agents) {
        let neighborDist = 50;
        let sum = createVector(0, 0);
        let count = 0;
        
        for (let other of agents) {
            let d = p5.Vector.dist(this.position, other.position);
            if (d > 0 && d < neighborDist) {
                sum.add(other.position);
                count++;
            }
        }
        
        if (count > 0) {
            sum.div(count);
            return this.seek(sum);
        }
        return createVector(0, 0);
    }
    
    // Buscar un objetivo
    seek(target) {
        let desired = p5.Vector.sub(target, this.position);
        desired.normalize();
        desired.mult(this.maxSpeed);
        
        let steer = p5.Vector.sub(desired, this.velocity);
        steer.limit(this.maxForce);
        return steer;
    }
    
    // Aplicar miedo a las ondas
    applyFear(waves) {
        this.fear = 0;
        let fearForce = createVector(0, 0);
        
        for (let wave of waves) {
            let d = p5.Vector.dist(this.position, wave.position);
            if (d < wave.radius) {
                // Calcular nivel de miedo según el estado
                let fearLevel;
                switch(state) {
                    case 1: // Tranquilo - mucho miedo
                        fearLevel = 1.5;
                        break;
                    case 2: // Bueno - miedo moderado
                        fearLevel = 1.0;
                        break;
                    case 3: // Intenso - poco miedo
                        fearLevel = 0.3;
                        break;
                    default:
                        fearLevel = 1.0;
                }
                
                this.fear = max(this.fear, fearLevel * (1 - d / wave.radius));
                
                // Si la onda toca al agente, cambiar su color
                if (d < wave.radius * 0.1) {
                    this.color = color(random(255), random(255), random(255));
                }
                
                // Calcular fuerza de repulsión
                let repulse = p5.Vector.sub(this.position, wave.position);
                repulse.normalize();
                repulse.mult(fearLevel * (1 - d / wave.radius) * 2);
                fearForce.add(repulse);
            }
        }
        
        // Aplicar fuerza de miedo
        if (this.fear > 0) {
            fearForce.limit(this.maxForce * 2);
            this.acceleration.add(fearForce);
        }
    }
    
    // Actualizar posición
    update() {
        // Actualizar velocidad máxima según el modo
        this.maxSpeed = this.getMaxSpeed();
        
        // Consultar campo de vectores
        let x = floor(this.position.x / fieldSize);
        let y = floor(this.position.y / fieldSize);
        x = constrain(x, 0, cols - 1);
        y = constrain(y, 0, rows - 1);
        
        let fieldForce = field[x][y].copy();
        fieldForce.mult(0.5);
        this.acceleration.add(fieldForce);
        
        // Actualizar velocidad y posición
        this.velocity.add(this.acceleration);
        this.velocity.limit(this.maxSpeed);
        this.position.add(this.velocity);
        this.acceleration.mult(0);
        
        // Rebote en los bordes
        if (this.position.x < 0) this.position.x = width;
        if (this.position.x > width) this.position.x = 0;
        if (this.position.y < 0) this.position.y = height;
        if (this.position.y > height) this.position.y = 0;
    }
    
    // Mostrar agente
    display() {
        push();
        translate(this.position.x, this.position.y);
        rotate(this.velocity.heading());
        
        // Color según el miedo
        if (this.fear > 0) {
            fill(255, 0, 0, 200); // Rojo cuando tiene miedo
        } else {
            fill(this.color);
        }
        
        noStroke();
        triangle(
            this.size, 0,
            -this.size, -this.size/2,
            -this.size, this.size/2
        );
        pop();
    }
}

// Clase Onda de Miedo
class Wave {
    constructor(x, y) {
        this.position = createVector(x, y);
        this.radius = 5;
        this.maxRadius = 300;
        this.life = 1.0; // 1 segundo de vida
        this.speed = 2;
    }
    
    update() {
        this.radius += this.speed;
        this.life -= 1.0 / (60 * 1.0); // 60 FPS por 1 segundo
    }
    
    display() {
        push();
        noFill();
        stroke(255, 255, 0, this.life * 255);
        strokeWeight(2);
        ellipse(this.position.x, this.position.y, this.radius * 2);
        pop();
    }
    
    isDead() {
        return this.life <= 0 || this.radius > this.maxRadius;
    }
}

// Clase Onda de Bajo
class BassWave {
    constructor(x, y) {
        this.position = createVector(x, y);
        this.radius = 5;
        this.maxRadius = min(width, height);
        this.life = 1.0;
        this.speed = 10;
    }
    
    update() {
        this.radius += this.speed;
        this.life -= 1.0 / (60 * 1.0);
    }
    
    display() {
        push();
        noFill();
        stroke(255, 255, 255, this.life * 100);
        strokeWeight(3);
        ellipse(this.position.x, this.position.y, this.radius * 2);
        pop();
    }
    
    isDead() {
        return this.life <= 0 || this.radius > this.maxRadius;
    }
}

// Clase Rastro
class Trail {
    constructor(x, y, col) {
        this.position = createVector(x, y);
        this.color = col;
        this.life = 1.0;
        this.size = 8;
    }
    
    update() {
        this.life -= 0.02; // Se desvanece más rápido
        this.size *= 0.95;
    }
    
    display() {
        push();
        fill(red(this.color), green(this.color), blue(this.color), this.life * 150);
        noStroke();
        ellipse(this.position.x, this.position.y, this.size);
        pop();
    }
    
    isDead() {
        return this.life <= 0 || this.size < 1;
    }
}

// Manejo de teclas
function keyPressed() {
    // Cambiar estados
    if (key === '1') {
        state = 1;
        setStateColors();
    } else if (key === '2') {
        state = 2;
        setStateColors();
    } else if (key === '3') {
        state = 3;
        setStateColors();
    }
    
    // Cambiar velocidad
    if (key === 'i' || key === 'I') {
        speedMode = 1;
    } else if (key === 'o' || key === 'O') {
        speedMode = 2;
    } else if (key === 'p' || key === 'P') {
        speedMode = 3;
    }
    
    // Generar ondas de miedo
    if (key === 'q' || key === 'Q') {
        for (let i = 0; i < 3; i++) {
            waves.push(new Wave(random(width), random(height)));
        }
    }
    
    // Activar/desactivar rastro
    if (key === ' ') {
        trailEnabled = !trailEnabled;
        if (!trailEnabled) {
            trails = []; // Limpiar rastros existentes
        }
        return false; // Prevenir scroll
    }
}

// Regenerar campo con ruido Perlin al hacer clic
function mousePressed() {
    if (mouseButton === LEFT) {
        generateField();
    }
}

```
  
</details>

### foto de la prueba 
<img width="760" height="502" alt="image" src="https://github.com/user-attachments/assets/8643eb7f-9bb0-4a7c-9605-9763d4ccd84f" />


Autoevaluación

Nota final: 4.5 Porque me faltaron 2 preguntas de varias actividades. Creo que el trabajo final esta completo y muy bien realizado con todo su pre diseño y entendimiento

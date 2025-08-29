# Unidad 1


## 🛠 Fase: Apply

### Activiad 8

__Pregunta__: Explica tu obra generativa

__Respuesta__: Quería hacer un efecto como de un efecto psicodelico, algo que se sintierá tanto como las drogas que consumes y te hacen sentir menos (cuando el mouse esta abajo) junto a los que te hacen sentir demasiado (el mouse arriba), la idea era que cuando tienes el mouse arriba representa caos, hay más saltos por parte de las bolas, se mueven de forma más rápida, los colores estan saturados. 

Por otro lado, cuando esta el mouse en la parte inferior se siente más apagado todo, los colores de las esferas estan menos saturadas, el movimiento es más lento, las esferas dan muchos menos saltos.


Código 

<details> 

<summary> Aquí esta el código </summary>

```javascript

// Variables globales para el nuevo comportamiento
let noiseTimeX = 0;
let noiseTimeY = 1000;
let bgColor = 0;
let bgHue = 0;

class Body {
    constructor(x, y) {
        this.x = x;
        this.y = y;
        this.size = random(10, 30);
        this.speedX = random(-2, 2);
        this.speedY = random(-2, 2);
        this.color = color(random(255), random(255), random(255));
        this.baseColor = this.color; // Guardar color original
        this.grayAmount = 0; // Cantidad de gris (0-255)

        // Variables para ruido Perlin únicas por esfera
        this.noiseOffsetX = random(1000);
        this.noiseOffsetY = random(2000);
    }

    move() {
        // 1. Ruido Perlin al movimiento (suave y orgánico)
        let noiseX = map(noise(this.noiseOffsetX), 0, 1, -1, 1);
        let noiseY = map(noise(this.noiseOffsetY), 0, 1, -1, 1);
        
        this.noiseOffsetX += 0.01;
        this.noiseOffsetY += 0.01;

        // 2. Velocidad basada en la posición Y del mouse
        let mouseYFactor = map(mouseY, 0, height, 1.5, 0.5); // Más alto = más rápido
        let currentSpeedX = this.speedX * mouseYFactor;
        let currentSpeedY = this.speedY * mouseYFactor;

        // Aplicar movimiento base + ruido Perlin
        this.x += currentSpeedX + noiseX;
        this.y += currentSpeedY + noiseY;

        // 2. Saltos probabilísticos basados en la posición Y del mouse
        let jumpProbability = map(mouseY, 0, height, 0.10, 0.005); // 10% a 0.5%
        if (random() < jumpProbability) {
            let jumpSize = map(mouseY, 0, height, 15, 3); // Saltos más grandes arriba
            this.x += random(-jumpSize, jumpSize);
            this.y += random(-jumpSize, jumpSize);
        }

        // Rebotes en los bordes
        if (this.x < 0) this.x = width;
        if (this.x > width) this.x = 0;
        if (this.y < 0) this.y = height;
        if (this.y > height) this.y = 0;
    }

    display() {
        // 3. Color basado en la posición Y del mouse
        let grayFactor = map(mouseY, height, 0, 0.8, 0); // Más abajo = más gris (0-0.8)
        
        // Obtener componentes RGB del color base
        let r = red(this.baseColor);
        let g = green(this.baseColor);
        let b = blue(this.baseColor);
        
        // Mezclar con gris según la posición del mouse
        let grayValue = 128;
        r = lerp(r, grayValue, grayFactor);
        g = lerp(g, grayValue, grayFactor);
        b = lerp(b, grayValue, grayFactor);
        
        // Crear el color final CORRECTAMENTE
        this.color = color(r, g, b, 150); // Alpha de 150 directamente
        
        fill(this.color);
        noStroke();
        ellipse(this.x, this.y, this.size);
    }
}

let bodies = [];

function setup() {
    createCanvas(800, 600);
    colorMode(RGB, 255, 255, 255, 255);
    
    // Crear esferas iniciales
    for (let i = 0; i < 20; i++) {
        bodies.push(new Body(random(width), random(height)));
    }
}

function draw() {
    // 4. Fondo que cambia de color con el tiempo - FORMA SEGURA
    bgHue = (bgHue + 0.5) % 360;
    
    // Usar HSB para el fondo pero de forma controlada
    colorMode(HSB, 360, 100, 100);
    let bgSaturation = map(sin(frameCount * 0.01), -1, 1, 30, 70);
    let bgBrightness = map(cos(frameCount * 0.02), -1, 1, 20, 40);
    
    background(bgHue, bgSaturation, bgBrightness);
    
    // Volver a RGB para las esferas
    colorMode(RGB, 255, 255, 255, 255);
    
    // Actualizar y mostrar todas las esferas
    for (let body of bodies) {
        body.move();
        body.display();
    }
    
    
}

function mousePressed() {
    // Añadir nueva esfera al hacer click
    bodies.push(new Body(mouseX, mouseY));
}

function keyPressed() {
    // Limpiar esferas con la tecla 'c'
    if (key === 'c' || key === 'C') {
        bodies = [];
    }
}

```
</details> 

[Enlance a P5.js](https://editor.p5js.org/gokuru12/sketches/P8gUaZh30)

Fotos

Cuando esta en alta 

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/e58e1400-4528-4a3c-babb-14b6f610e397" />

Cuando esta bajo

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/03e01133-4844-4411-bb62-338ea55a5706" />


# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1 
_Pregunta_: Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo.

_Respuesta_: La aletoriedad permite crear experiencias que sean cambiantes pero sin la necesidad de cambiar la profundidad o hacer una nueva creación

### Actividad 2
_Pregunta_: Cuál es el papel de la aleatoriedad en su obra.
Según tu perfil profesional, cómo se aplica el concepto de aleatoriedad en el tipo de proyectos que desarrollas. Ilustra tu respuesta con ejemplos concretos.

_Respuesta_: La aletoriedad hace que se vuelva himnotizante la música porque uno no sabe que esperar sobre como se verá la imagen, vuelve la imagen impredecible y por eso mismo, una experiencia novedosa.

Mi apartado en la carrera es el game design y el diseño de nivel, me encargo de que los videojuegos tengan un buen balance y se sientan como una gran experiencia divertida, una manera que se me ocurre para usar aletoriedad es creando generación de enemigos que cuenten con estadisticas parecidas pero que varien dependiendo de la situación junto a sus modelos, esto haría que cada nivel se observarian muchos tipos de enemigos y manejarían estadisticas varias pero sin dar saltos grandes o problemas muy grandes.

### Actividad 3
_Pregunta_: Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.
Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.
Ejecuta el código y escribe en tu bitácora qué sucedió realmente.
Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?

_Respuesta_: Modifique en walker el contructor en vez de la mitad, puse 1 de ancho y 3 de alto, puse el background en azul al iniciar en azul y con transparencia de 8, tambien agrege una segunda figura, agrege otro walker llamado walker2 que es un circulo, cambie el background a azul, una coma y un 8 para que sea algo transparente el fondo

### Actividad 4
_pregunta_: ¿Cual es la diferencia entre distribución uniforme y no uniforme?

_Respuesta_: La distribución uniforme es cuando hay la misma probabilidad de que ocurran los eventos, en la no uniforme las posibilidades no son la misma, un ejemplo sería que haya probabilidad de 1% de que en vez de moverse aleatoriamente haga el movimiento pero en un salto largo, no es la misma probabilidad.

_Pregunta_: Cambiar el valor para que la no uniforme salga más a la derecha

_Respuesta_: Cambiar los valores de randomGaussian entre valores cercanos a 100 y 80

### Actividad 5
_Pregunta_: Coloca el link del sketch, el codigo del sketch y una captura de pantalla

_Respuestas_:
Foto: <img width="939" height="309" alt="image" src="https://github.com/user-attachments/assets/6604a1e0-9cb4-4aa1-8e5e-ef561cf15571" />

Link: https://editor.p5js.org/gokuru12/sketches/rYFvl2nz8

Codigo: function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);
}

function draw() {
  let x = randomGaussian(360, 50);
  noStroke();
  fill(20, 10);
  circle(x, 120, 16);
}

### Actividad 6
_Pregunta_: Que técnica usaste para hacer el efecto, explica los cambios que hiciste y que esperabas, copia el link, el código y la foto del sketch

_Respuesta_: Lo que hice fue modificar mi prueba de caminata aleatoria cambiando el metodo "step" cambiando el uso de toma de decisión de que dirección ir por la probabilidades de tomar un paso aleatorio teniendo en cuenta un random que primero toma la probabilidad de que ocurra el evento, si no ocurre hace un paso norma, si ocurre hace el paso largo.

Foto: <img width="1858" height="701" alt="image" src="https://github.com/user-attachments/assets/365a4bfb-2b76-42ea-8a52-e06b1479e376" />

Link: https://editor.p5js.org/gokuru12/sketches/dK54fPmu1

Código: let walker;
let walker2;

function setup() {
  createCanvas(640, 240);
  background('blue');
  walker = new Walker();
  walker2 = new Walker();
  
}

function draw() {
  walker.step();
  walker.show();
  
  walker2.step();
  walker2.show2();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 3;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }
  
  show2() {
    stroke(0);
    circle(this.x, this.y,10);
  }

  step() {
    let r = random(1);
    let xstep, ystep;
    
    if (r < 0.01){
      xstep = random(-100,100);
      ystep = random(-100,100);
      
    }
    else {
      xstep = random(-1, 1);
      ystep = random(-1, 1);
    }
    this.x += xstep;
    this.y += ystep;
  }
}

### Actividad 7
_Pregunta_: Prueba hacer un cambio en la demostración de ruido perlin y sube la foto, el link y el código.

_Respuesta_: Lo que esperaba hacer era que se moviera tambien hacia arriba y hacía abajo junto a los movimientos laterales, el resultado fue que el ruido empezó a caer poco a poco.

Foto: <img width="340" height="228" alt="Captura de pantalla 2025-08-18 191739" src="https://github.com/user-attachments/assets/6c7dda38-a852-4cfb-bb00-f5c71d8289af" />

Link: https://editor.p5js.org/gokuru12/sketches/xcWiU5tH4

Código: 
let t = 0;
let y = 0;

function draw() {
  let n = noise(t);
  let m = noise(y);
  //{!1} Use map() to customize the range of Perlin noise.
  let x = map(n, 0, 1, 0, width);
  let p = map(m, 0, 1, 0, height);
  ellipse(x, y, 16, 16);
  //{!1} Move forward in time.
  t += 0.01;
  y += 0.01;
}





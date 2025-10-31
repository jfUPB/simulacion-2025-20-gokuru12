# Evidencias de la unidad 7


### Actividad 1


__Cuales fueron 3-4 ejemplos que fueron interesantes para mi?__: Hay varios cuanto menos creativos e interesantes, mis favoritos estan entre: Condón, es gracioso ver como se estira la C. El "tunnel" donde el tunel en si son las 2 n haciendo el efecto del camino de un tunel. Y el de "inflation" poniendo muchas o simulando 0´s como cuando las cosas aumentan su precio me pareció una idea buenisima 

__Que ejemplos (sin animación) se te ocurren__: En la palabra viento quise hacer como si las letras recibieran una rafaga de viento y los objetos estan pegados al piso pero se ve el efecto en la parte superior, como un estiron. El otro es "libre" que quise hacer que la L tuviera como amarrado al resto de la palabra y luego se retrae para dejarlos... libre

<img width="587" height="501" alt="image" src="https://github.com/user-attachments/assets/bb924e2d-9361-462c-ad47-938778aefc54" />


### Actividad 2


__Para que se usa ¨Engine¨, ¨World¨, ¨Bodies¨, ¨Constraint¨ y ¨MouseConstraint¨?__: 

Engine: Es el apartado que trae el motor que calcula las físicas de una vez

World: Es el apartado que pone que lo que salga en pantalla es el "mundo" y en ese mundo es donde ocurren las situaciones, por lo que aplique en la programación

Bodies: Son los objetos que se ven afectados por las relgas físicas del engine. 

Constraint: Es la forma de decirle las restricciones a los objetos, es lo que le da las instrucciones de como funcionar a los objetos.

MouseConstraint: Es el conjunto de cosas que permite al mouse interactuar con los objetos, es lo que permite al mouse hacer click a objetos y mover, jalar, entre otras cosas. 

### Actividad 3 

__¿De que trata la experiencia?__: Use la palabra **"Colgar"** queriendo hacer una clase de pendulo, la palabra colgar esta colgandose

__¿Cómo funcionaría?__: El movimiento que tiene principalmente es de la caida con la gravedad y junto a eso el movimiento perpendicular que forma cuando mueves un objeto colgando. El usuario podría hacerle click y mover la palabra para hacerlo moverse y que se cuelgue y balancee

__Imagen de referencia__: 

<img width="860" height="473" alt="Captura de pantalla 2025-10-17 074207" src="https://github.com/user-attachments/assets/0dc50e65-a22d-4fc4-848c-11185ce31abc" />



<details>

<summary> Aquí esta el código </summary>

```javascript


let Engine = Matter.Engine,
    Render = Matter.Render,
    World = Matter.World,
    Bodies = Matter.Bodies,
    Constraint = Matter.Constraint,
    Mouse = Matter.Mouse,
    MouseConstraint = Matter.MouseConstraint;

let engine;
let world;
let wordBody;
let stringConstraint;

function setup() {
  let canvas = createCanvas(800, 500);
  engine = Engine.create();
  world = engine.world;

  // Fondo estilo "cielo"
  background(180, 220, 255);

  // Crear el cuerpo de la palabra (un rectángulo que representa el texto)
  let wordWidth = 120;
  let wordHeight = 40;
  wordBody = Bodies.rectangle(width / 2, 200, wordWidth, wordHeight, {
    restitution: 0.4,
    friction: 0.3,
  });
  World.add(world, wordBody);

  // Crear una "cuerda" que sostiene el cuerpo desde arriba (un constraint)
  stringConstraint = Constraint.create({
    pointA: { x: width / 2, y: 50 }, // punto fijo en el aire
    bodyB: wordBody,
    pointB: { x: 0, y: -wordHeight / 2 }, // se une desde arriba del rectángulo
    stiffness: 0.05, // elasticidad de la cuerda
    length: 150, // largo de la cuerda
  });
  World.add(world, stringConstraint);

  // Habilitar interacción con el mouse
  const canvasMouse = Mouse.create(canvas.elt);
  const mouseConstraint = MouseConstraint.create(engine, {
    mouse: canvasMouse,
  });
  World.add(world, mouseConstraint);

  // Iniciar el motor físico
  Engine.run(engine);
}

function draw() {
  background(180, 220, 255); // fondo azul cielo

  // Dibujar la cuerda
  stroke(60);
  strokeWeight(2);
  line(
    stringConstraint.pointA.x,
    stringConstraint.pointA.y,
    wordBody.position.x,
    wordBody.position.y - 20
  );

  // Dibujar la palabra "colgar"
  push();
  translate(wordBody.position.x, wordBody.position.y);
  rotate(wordBody.angle);
  noStroke();
  fill(30);
  textAlign(CENTER, CENTER);
  textSize(32);
  text("colgar", 0, 0);
  pop();
}

```

</details>


__Foto del resultado final__

<img width="463" height="305" alt="Captura de pantalla 2025-10-17 074630" src="https://github.com/user-attachments/assets/db53c9e8-a5be-487d-993d-9464e7bd20bf" />

[Enlace al proyecto](https://editor.p5js.org/gokuru12/sketches/jhDpuq58M)


__Autoevaluación__: El proyecto lastimosamente no funcionó correctamente, el arrastre del click que tenía que hacer mover la palabra no funciono, aunque use el mouseconstrain aun así no funcionó como espera, la verdad a pesar de trabajar e intentar conseguir el error no lo conseguí corregir, pero si cae con la gravedad al inicio cuando se prende el programa

Por otro lado las actividades hice la primera completa y la segunda me falto una de las actividades, por lo que se que eso se refleja en mi nota

Nota: 3.8

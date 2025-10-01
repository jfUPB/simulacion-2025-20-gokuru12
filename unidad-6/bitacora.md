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

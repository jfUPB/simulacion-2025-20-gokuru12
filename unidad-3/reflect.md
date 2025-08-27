# Unidad 3


## 🤔 Fase: Reflect

### Actividad 11

#### Parte 1 

#### 1. 
__Pregunta__: Escribe la ecuación vectorial de la segunda ley de newton y explica cada uno de sus componentes.

__Respuesta__: La formula es  **∑F = m*a**  donde el significado es que la sumatoria de todas las fuerzas es igual a la masa por la aceleración.

Pero estamos hablando de vectores así que aquí aplica algo un poco distinto y son los vectores de la aceleración y de las fuerzas, en este caso se usaría una formula como la siguiente:

**∑F → x = m*ax → x / ∑F → y = m*ay → y / y ∑F → z = m*az → z**
<details>
<summary> <ins>Explicación cada componente</ins> </summary>

__∑F__: Es la sumatoria de fuerzas considerando los vectores, tanto en x, y e z.

__m__: La masa del objeto, aplicando una fuerza hacia el suelo por la gravedad

__ax__: El componente de la aceleración en el vector X

__ay__: El componente de la aceleración en el vector Y

__az__: El componente de la aceleración en el vector Z

</details>

#### 2. 
__Pregunta__: ¿Por qué es necesario multiplicar la aceleración por cero en cada frame del método update()?

__Respuesta__: El motivo es que en cada frame se hace una suma de la velocidad anterior con aceleración, si no se borra la aceleración se vuelve un acumulativo de todas las velocidades anteriores y volviendola una aceleración que va escalando en vez de ir cambiando respecto a los valores reales.

#### 3. 
__Pregunta__: Explica la diferencia entre paso por valor y paso por referencia cuando aplicamos fuerzas a un objeto.

__Respuesta__: Cuando modificamos el valor de una fuerza normalmente es porque hicimos un copy del valor de la referencia, haciendo así que la referencia no cambie y la podamos seguir usando con su valor origial. 

Si modificamos la referencia, lo que ocurre es que el valor en si cambia por lo que si necesito que mantenga un valor todo el tiempo para causar el efecto que necesito al momento de modificarlo ya no hará lo que deseo.

#### 4.
__Pregunta__: ¿Cuál es la diferencia conceptual entre modelar fuerzas (como fricción, gravedad) y simplemente definir algoritmos de aceleración?

__Respuesta__: La diferencia principal es que al crear fuerzas podemos usar o causar reglas especificas para que ocurran efectos interesantes sobre nuestros proyectos visuales, los vuelve más dinamicos y realistas además de entregarles una gran experiencia al usuario con productos interactuables. 

Junto a eso, estan multiples casos útiles en cosas como videojuegos el saber hacer simulaciones físicas que permitan hacer más realistas las cosas y no solo moverse más o menos rápido.

#### Parte 2

#### 1.

__Pregunta__: ¿que fue lo más desafiante?

__Respuesta__: Intentar hacerlo interactuable, me parecía un concepto muy lindo por si solo entonces modificarlo me era raro, al final sentí que jugando con "ser el creador" y tomar la decisión de quien tiene más fuerza y cuantas esferas habrá me resulto satisfactorio

#### 2.

__Pregunta__: ¿Ocurrió alguna sorpresa o algo que no esperaba?

__Respuesta__: Por un momento quise aplicar el ruido perlin para que los movimientos fueran más dinamicos, pero en vez de conseguir un movimiento semi caotico logré que las esferas desaparecieran y no pudiera controlarlas en lo absoluto, tuve que retractarme de esas decisión

#### 3.

__Pregunta__: ¿Que ha cambiado sobre mi percepción del arte generativo?

__Respuesta__: La interación de la física para hacer algo que no es ajeno a nosotros pero que se puede modificar y entener es genial, especialmente cuando tenemos control de lo que se hace y se cambia, se siente como una forma de expresar algo pero de manera no tan literal o no con un estilo clasico, es gratificante crear algo así

#### 4.

__Pregunta__: ¿Si tuviera otra semana más que le aplicaría?

__Respuesta__: Seguramente fricción en ciertos lugares para que se sienta más complejo cuando van por ciertos lugares, junto a lo mejor rebotes entre ellos, creo que sería algo divertido ver esas reacciones.

### Actividad 13

#### 1. Continuar

__Pregunta__: ¿Qué actividad o concepto de esta unidad te resultó más “revelador” para entender las fuerzas en el arte generativo?

__Respuesta__: El de los n cuerpos, siento que se puede hacer muchas cosas con solo ese ejemplo que si se piensa bien desemboca en muchas posibilidades (aúnque para mi no fue tan fácil pensar en formas de modificarlo)

#### 2. Dejar de hacer

__Pregunta__: ¿Hubo alguna actividad que te pareció redundante o menos efectiva para comprender el modelado de fuerzas?

__Respuesta__: La actividad 4, especialmente porque aunque sirve como un abrebocas para entender que harémos a mi me dejo un poco más confundido que otra cosa

#### 3. Progresión conceptual

__Pregunta__: El paso de manipular aceleración directamente (unidad 2) a modelar fuerzas (unidad 3) te pareció una progresión natural y efectiva? ¿Por qué?

__Respuesta__: No lo se muy bien porque en este punto exacto no he terminado la unidad 2 (lo siento profe, este fin de semana me corte el pie con un clavo oxidado y tuve que buscar en todos lados donde me atendieran para la vacuna, perdí todo el fin de semana en eso) 

#### 4. Conexión arte-física

__Pregunta__: ¿Cómo te ha ayudado esta unidad a ver la conexión entre conceptos físicos y expresión artística? ¿Te sientes más cómodo “jugando” con las leyes de la física en tus creaciones?

__Respuesta__: Bastante más, me hace pensar que la física no solo sirve para hacer que algo funcione como en los videojuegos que solo lo usamos para que se vea realista, puede usarse para crear algo bello

#### 5. Comentario adicional

__Pregunta__: ¿Algo más que quieras compartir sobre tu experiencia explorando fuerzas en el arte generativo?

__Respuesta__: Ojala haber tenido más tiempo y recursos por mi parte para explorar más sobre las cosas que se podían aplicar en terminos de arte, me suelo centrar mucho en que las cosas funcionen y suelo descuidar lo estetico, me gustó mi idea pero me hubiera gustado darle más narrativa de toda la situación

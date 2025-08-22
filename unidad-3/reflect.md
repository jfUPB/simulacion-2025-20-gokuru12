# Unidad 3


## 🤔 Fase: Reflect

### Parte 1 

#### 1. 
__Pregunta__: Escribe la ecuación vectorial de la segunda ley de newton y explica cada uno de sus componentes.

__Respuesta__: La formula es  **∑F = m*a**  donde el significado es que la sumatoria de todas las fuerzas es igual a la masa por la aceleración.

Pero estamos hablando de vectores así que aquí aplica algo un poco distinto y son los vectores de la aceleración y de las fuerzas, en este caso se usaría una formula como la siguiente:

**∑F → x = m*ax → x / ∑F → y = m*ay → y / y ∑F → z = m*az → z**
<details>
<summary> Explicación cada componente </summary>

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

### Parte 2

# Investigación

### Actividad 2

__Que esta ocurriendo en esta simulación? Y cual es la interacción?__: Es una linea recta que cuando le das a una tecla empieza a rotar con el punto de eje en el centro, el centro de graverdad esta en toda la mitad, cada vez que le das a la tecla rota

__Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?__: Creo que debe ser para asegurarse que siempre este en el centro como si fuera su centro de gravedad, ya que la rotación movería del centro a la linea

__Cuál es la relación entre el sistema de coordenadas y la función rotate().__ La rotación en si no hace que se rote el objeto, solo cambia el angulo, con el sistemas de cordenadas le dices en que lugar debe rotar, sino cambiaria de lugar


__¿Qué hace la función heading()?__: Es el que tiene la información de hacia donde esta el vector de algo, junto a su velocidad (creo)
__¿Qué hace la función push() y pop()?__ El push guarda la información sobre los objetos, como rotación, posición entre otras cosas. El pop devuelve el estado en el que estaba cuando se hizo el push.
__¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.__: Hace que del objeto que este se considere su centro... como el centro, haciendo que si rota, o quiere moverlo se considere desde ese punto.
__¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.__

Objetivo de la experiencia: Mi objetivo es hacer un pendulo que tenga dos cualidades, una que si le doy clic en un lugar, cambie el centro del origen del pendulo (el inicio de la cuerda) a donde le haya hecho click, y cambie el lugar del origen, además de eso que haya la posibilidad de al darle a los botones laterales aplique una fuerza que mueva el pendulo

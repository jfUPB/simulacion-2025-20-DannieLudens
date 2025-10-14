# Unidad 3

## 🤔 Fase: Reflect

### Actividad 11 Autoevaluación

#### Parte 1: recuperación de conocimiento (Retrieval Practice)

1. Escribe la ecuación vectorial de la segunda ley de Newton y explica cada uno de sus componentes.

> F = m*a . F es la fuerza o suma de fuerzas. m es la masa del cuerpo y a es la aceleracion y es como cambia la velocidad a traves del tiempo

2. ¿Por qué es necesario multiplicar la aceleración por cero en cada frame del método update()?

> Porque sino se hace esto se acumulan las fuerzas y esto es poco realista y no ocurre.. hay que recordar el ejemplo de la patineta cada patada que se da para impulsarse el momento justo despues es como hacer esa multiplicacion por cero porque sino seria como si esa aceleracion que se aquirio en la ultima patada fuera persistente y asi suceviamente con las proximas patadas para impulsarse lo que ocasionaria que saliera disparado de manera exponecial.

3. Explica la diferencia entre paso por valor y paso por referencia cuando aplicamos fuerzas a un objeto.

> El paso por valor es como si usaramos una copia del valor mientras que el paso por referencia es como si usaramos el valor original.
  
4. ¿Cuál es la diferencia conceptual entre modelar fuerzas (como fricción, gravedad) y simplemente definir algoritmos de aceleración?

> No lo recuerdo, debo consultarlo pero se me ocurre que modelar fuerzas como las mencionadas permite establecer reglas realistas basados en fisicas para exprimentar dentro de prototipos mientras que definir algoritmos de aceleracion solo le dan un valor predefinido a un objeto para que se mueva

#### Parte 2: reflexión sobre tu proceso (Metacognición)

1. ¿Qué fue lo más desafiante en la Actividad 10 (problema de los n-cuerpos)? ¿El concepto creativo, la implementación de las fuerzas o la integración de la interactividad?

> Para mi fue la conceptualización de la union de los dos ele,entos Calder con los N-Cuerpos, porque auqnue se suponia que era crear una obra generativa inspirado por los dos.. para mi cerebro no tan creativo todo lo que proponia me llevaba a Calder... despues de casi 3 semanas con ayuda del profe retome esa aplicacion pensandola mas en su equilibrio y colores y con ayuda de una exploracion que habia hecho en la unidad 4 "ramitas" fue que pude aterrizar la idea.. sin embargo no quedé contento porque aunque si implemente el equilibrio... es un movil de Calder afectado por el problema de los N cuerpos y el viento...

2. ¿Las fuerzas que modelaste produjeron el comportamiento que esperabas? Describe un momento “sorpresa” (esperado o inesperado) durante el desarrollo.

> Si produjeron el comportamiento esperado sin embargo mi sorpresa o lo inesperado fue en la experimentacion ver como estos pendulos al estar atraidos todos hacia todos 5 figuras hacia cada una de las demas es decir 4 atracciones para cada una al aumentarles la fuerza gravitacional y el hecho de que estuvieran amarradas, hizo que lo que se dibujara en el canvas secundario fuera interesante de ver y es lo que me parecio que lo hace insignia 

3. ¿Cómo ha cambiado tu forma de pensar sobre la “física” en el arte generativo después de esta unidad?

> Manipular la fisica para generar obras generativas se ve prometedor, y deja de ser tan miedoso y se vuelve algo mas divertido de implementar y experimentar.

4. Si tuvieras una semana más, ¿Qué otras fuerzas te gustaría modelar o cómo mejorarías tu simulación del problema de los n-cuerpos?
 
> Honestamente, no solo me tome una semana mas, sino que me tome casi 4 semanas pero no trabajandole solamente a este problema de n cuerpos y Calder..que para mi fue complejo y no deberia serlo porque sobrepienso mucho las cosas, pero me gustaria poder mejorarlo en un entorno 3D en p5js para ver como la fuerza centrifuga y otros elementos derivados de las figuras haria mover los elementos en el espacio y siento que se pareceria mucho a un modelo de planetario real, seria como proponer otra galaxia


### Actividad 12 Coevaluación

Coevaluacion con Parra no la pudimos realizar

### Actividad 13 Feedback

1. Continuar: ¿Qué actividad o concepto de esta unidad te resultó más “revelador” para entender las fuerzas en el arte generativo?
Dejar de hacer: ¿Hubo alguna actividad que te pareció redundante o menos efectiva para comprender el modelado de fuerzas?

> El Marco Motion 101 para mi fue el que abrió las puertas para entender la base de cualquier obra generativa que vaya a construir que tenga movimiento este seria como el manual para principiante de como contruirlo paso a paso. Siento que el tema de los N cuerpos de la unidad de fuerzas deberia tener un poco mas de actividades propias, sin emabrgo entiendo que al estar en el apply y tener mas tiempo para estudiarla lo compensa para su exploración. 

2. Progresión conceptual: ¿El paso de manipular aceleración directamente (unidad 2) a modelar fuerzas (unidad 3) te pareció una progresión natural y efectiva? ¿Por qué?

> Si, siento que asi como se aprende a montar bicicleta primero se arrastra uno con los pies y luego con los pedales progresivamente es la manera correcta como se esta presentando la guia, ademas en cursos anteriores ya veniamos manipulando la aceleracion y algoritmos pero no era como que los modelaramos basados en fisica conscientemente para p5js con obras generativas como hacemos ahora (si modelabamos en python en modelamiento matematico para modelar fisicas de sistemas como puentes colgantes o catapultas)
   
3. Conexión arte-física: ¿Cómo te ha ayudado esta unidad a ver la conexión entre conceptos físicos y expresión artística? ¿Te sientes más cómodo “jugando” con las leyes de la física en tus creaciones?

> Sí me siento mas comodo, porque puedo imaginar y manipular en la expriemntacion de manera mas fluida si por ejemplo, entiendo que lo que esta afectando a mi objeto en la simulacion es el viento (algo fisico que se puede modelar) y quiero que sea el viento lo que afecta en mi simulacion.
   
4. Comentario adicional: ¿Algo más que quieras compartir sobre tu experiencia explorando fuerzas en el arte generativo?

> Cada vez que experimento mas, surgen mas.. pero trato de resolverlas y docuemntarlas de la mejor manera para interiorisarlas... ver los videos asociados a cada tema dentro del libro de Daniel Shiffman ayuda demasiado, al igual que ejemplos como series existentes recientmente agregadas que tocan estos temas son interesantes pero escasos.. por ejemplo la seria de Netflix el problema de los 3 cuerpos 2024

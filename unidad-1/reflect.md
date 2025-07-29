# Unidad 1

## 🤔 Fase: Reflect

### Actividd 9

Parte 1

1. Diferencia fundamental entre la aleatoriedad generada por random() y la apariencia de aleatoriedad del ruido perlin Noise() en que tipo de situacion usarias cada una

Respuesta: La diferenencia fundamental entre estas dos es que para implementar el random es necesario restringir el rango de valores que se quieren aleatorias mientras que el ruido perlin es generado y aunque tambien se puede restringir su rango es mas utilizado para generar aleatoriedad que agarrar un numero aleatorio de la bolsa de numeros (por asi decirlo)

El Ruido perlin se puede usar para generar texturas por ejemplo en un objeto de blender, tambien se puede usar para generar noise para luego modular una onda y generar un sonido, tambien se puede usar para generar terrenos y oleaje, tambien para suavisar la curva de un conjunto de datos distribuidos no uniformes o uniformes

El Random se puede usar para elegir un rango de colores, tambien para elegir un numero entre muchos, pensandolo en proyectos seria elegir aleatoriamente un dato que puede estar dentro de un arreglo de manera aleatoria y tambien para elegir posiciones aleatorias

2. Con mis palabras, que es una distribucion de probabilidad y cual es la diferencia visual que produce uan caminata aleatoria con una distribucion unifrme y una con distribucion normal

Respuesta: La distribucion de probabilidad es cómo estan distribuidos los datos de manera que un dato es mas probable que otro con respecto a la media pero tambien dependen de que tipo de distribucion

la diferencia visual de un random walker con distribucion uniforme a otro con distribucion normal esque el walker en la normal se va a ver mucho mas concentrado caminando casi que en el mismo punto mientras que el walker uniforme podria tener ciertas tendencias a moverse en un espacio mas amplio pero concentrado

3. Cual es el papel de la aleatoriedad en el arte generativo

Respuesta: generar experiencias y comportamientos distintos cada vez que se experimente con la obra, genera sostenimiento de la obra en el tiempo ya que cada vez que se visite sera algo similar pero unico, asi como en la naturaleza permite diversidad (analogia del crecimiento de un arbol segun el entorno)

4. el concepto de la comida cayendo sobre la pecera utiliza la distribucion no uniforme lo que hace que funcione como si actuaran factores externos como el viento, la corriente de aire que empujo un granito de comida y asi, tambien el comportamiento de los peces cuando el pez controlado con el mouse que bien puede alejarse tranquilos o puede que se asusten y salgan disparados utilizando la el vuelo de Levy

5. Que es un paseo o caminata en el contexto de la simulacion, que caracteristica particular tiene una caminata de tipo Lvey flight

Respuesta: un walk en simulacion es un movimiento simulando una caminata en este contexto de aleatoriedad implica que el walker se movera de manera aleatoria en una de 4 direcciones (hasta mas) y la caractersitica principal que tiene la caminata con Levy esque hace pasos cortos y en algun momento puede hacer un paso grande y esto ocurrir de manera aleatoria

Parte 2:

1. Cual fue el concepto mas abstaracto o dificil de visualizar para mi en esta unidad y que hice para finalmente comprenderlo

Respuesta: La diferernciacion entre las distribuciones uniformes y distribucion normal lo que hice fue consultar porque me genera cierta confusion al ser muy parecidas:

La distribución uniforme asigna la misma probabilidad a todos los valores dentro de un rango específico, mientras que la distribución normal, también conocida como distribución de campana, concentra la probabilidad alrededor de la media, con valores más alejados de la media teniendo menor probabilidad. 

2. El error que encontre esque el comportamiento de los peces del cardumen no era el esperado pues lo que pretendia era que los peces se alejaran en direccion contraria de manera aleatoria en direccion al pez principal si se acercaba, lo que me represento un reto porque tenia que pensar o disenar un comportamiento para los peces dpendiendo de la posicicon del pez principal

3. Siento que la interaccion entre los sistemas pero no estoy muy seguro

4. combinaria mejor la aleatoriedad es decir que los peces fueran walkers aleatorios pero mas suavizado cuando el pez principal se acercara su comportamiento fuera de un walker levy flight y que los peces del cardumen nadaran hacia la comida que esta mas cerca de cada uno y lo consumiera volviendose un pez mas grande cada vez (nueva implementación)

### Actividad 10

Bitacora de Parra [](URL.com)

Giff de su obra generativa:



#### Comentario de retroaliemntacion constructiva para la Act8 de Parra

Su Obra generativa sobre fuegos artificiales mediante la implementacion de particulas que se dispersan de manera aleatoria desde el centro me parecio muy chevere, no solo por que aplico conceptos de probabilidad para el cambio de color o la distribucion de las particulas sino porque logró encapsular en un mismo concepto estos elementos para que el espectador tuviese una experiencia visual unica cada vez

El feedback que le dí fue que le podria agregar el concepto de distribucion no uniforme a la aparicion del fuego artificial para que no siempre fuera en el centro sino en un rango aleatorio horizontal y adicional que las particulas mientras se disiparan no se borraran inmeditamente al dar click para generar un nuevo fuego artificial sino que tambien se fueran desapareciendo lentamente de esta manera darle continuidad a la obra conservando los fuegos artificales anteriores



### Actividad 11

1. **Continuar:** Los ejemplos alternos a los mostrados en el libro para entender mejor como se usan esos conceptos en otros espacios, ayuda a mejorar la perpectiva y conectar con experiencias clave que podemos identificar, esto deberia mantenerse si o si.

   Adicionalmente los plazos de entrega establecidos este semestre suponen un reto que le agrega a esta gran receta no solo un objetivo o meta sino tambien un limite y de una u otra manera ayuda a tener cierta presion que acelera ese instinto o deseo de cumplir de buena manera (no se porque mi cerebro trabaja mejor bajo presion es como si me obligara a comprometerme más)

3. **Dejar de hacer:** Quizas lo unico que me parece que deberia dejar de hacerse en el curso seria dejar bloqueado la siguiente unidad mientras estamos en la unidad anterior por ejemplo en la unidad 1 llegamos al reflect y me gustaria trabajar en la unidad 2 para ir adelantando y no esperar al martes para empezar con el seek

    No hubo ningun Concepto que me pareciera redundate, confuso hasta el momento todo a sido muy claro y util

3. **Empezar a hacer:** Me hizo falta en la ultima clase ver las obras generativas de los compañeros tipo un showcase de todos, porque brinda panoramas de ideas de concepto que los demas han explorado

4. **Balance teoría-práctica:** Analizar los ejemplos del texto guia tanto de los ejemplos de la pagina del curso como de nature of code y la documentacion de p5js me han parecido muy utiles e ilustrativos sobre todo para entender los conceptos aplicados y visualmente la logica detras, siento que todavia en la practica me quedo mucho tiempo leyendo, modificando y haciendo preguntas a la IA sobre el funcionamiento de los ejemplos e ideas que se me van ocurriendo, lo cual no es util para las entregas agiles del curso

5. **Comentario adicional:** Por el momento me esta encantando la metodologia del curso, si bien empecé algo atrasado, es algo que no me estoy permitiendo más para mantener esa disciplina.

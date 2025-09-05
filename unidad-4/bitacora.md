# Evidencias de la unidad 4

## Set Seek

**Introducción** 📜

En las unidades anteriores has explorado el movimiento con el marco motion 101 manipulando la posición de los elementos gráficos. En esta unidad explorarás el movimiento angular u oscilatorio.

**Set: ¿Qué aprenderás en esta unidad?** 💡

- Comprender los conceptos básicos del movimiento angular y oscilatorio.
- Explorar cómo se pueden aplicar estos conceptos en la creación de obras generativas interactivas en tiempo real.

---

**Seek: Investigación** 🔎

Exploración y desarrollo de la unidad 4

### Actividades de la unidad 4

<details>
  <summary>Proceso de desarrollo y aprendizaje</summary><br>

<details>
  <summary>Actividad 1 Memo Akten</summary><br>
  
**Te presente a Memo Akten**
  
En esta actividad te presentaré a [Memo Akten](https://www.memo.tv/), un artista y programador que ha explorado las posibilidades de la inteligencia artificial en la creación de arte. Te voy a presentar una obra de Memo bajo el título sombrilla de [Simple Harmonic Motion](https://www.memo.tv/works/simple-harmonic-motion/).

Solo para los curiosos: dale una mirada a la obra [Superradiance](https://superradiance.net/). Te dejo por aquí un video reciente: SUPERRADIANCE. Chapters 1-2. Short (Performance) version. By Memo Akten & Katie Peyton Hofstadter. [Youtube](https://youtu.be/TkZnkyvGIGY?si=Vb4Jg3BcIWG_Clqa).

</details>

<details>
  <summary>Actividad 2 Conceptos Fundamentales </summary><br>

Analiza las siguientes simulaciones y responde las preguntas.

Te voy a proponer un par de simulaciones para que analices.

Primero mira esta simulación para el [manejo de ángulos](https://editor.p5js.org/juanferfranco/sketches/R1iTVQjzm).

- ¿Qué está pasando en esta simulación? ¿Cuál es la interacción?

  -

- Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?

  -
  
- Cuál es la relación entre el sistema de coordenadas y la función `rotate()`.

  - 

Nota esta parte del código:

```
  line(-50, 0, 50, 0);
  stroke(0);
  strokeWeight(2);
  fill(127);
  circle(50, 0, 16);
  circle(-50, 0, 16);
```

Observa que al dibujar los elementos gráficos parece que se está dibujando en la posición `(0, 0)` del sistema de coordenadas. ¿Por qué crees que se hace esto? y ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?

Ahora analiza una simulación que muestra cómo puedes hacer para que los elementos gráficos de la simulación [apunten en la dirección del movimiento](https://editor.p5js.org/natureofcode/sketches/bZqHGYbRQ).

- Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?

  - 

Observa detenidamente este fragmento de código de la simulación:

```js
  display() {
    let angle = this.velocity.heading();

    stroke(0);
    strokeWeight(2);
    fill(127);
    push();
    rectMode(CENTER);
    translate(this.position.x, this.position.y);
    rotate(angle);
    rect(0, 0, 30, 10);

    pop();
  }
```

- ¿Qué hace la función heading()?

  - 

- ¿Qué hace la función push() y pop()? Realiza algunos experimentos para entender su funcionamiento.

  -

- ¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.

  -

- ¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.

  -

</details>

<details>
  <summary>Actividad 3 Practicar un poco</summary><br>

Ahora es momento de practicar los conceptos anterior. Crea una simulación de un vehículo que puedas conducir por la pantalla utilizando las teclas de flecha: la flecha izquierda acelera el vehículo hacia la izquierda, y la flecha derecha acelera hacia la derecha. El vehículo tendrá forma triangular y debe apuntar en la dirección en la que se está moviendo actualmente.

</details>

<details>
  <summary>Actividad 4 Relacion con el marco motion 101</summary><br>

Es momento de retomar lo que has aprendido en las unidades previas e integrarlo con los nuevos conceptos de esta unidad. Observa detenidamente la siguiente simulación: [Motion 101 con fuerzas](https://editor.p5js.org/juanferfranco/sketches/jebkEAUpR)

Identifica motion 101. ¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas? Trata de recordar por qué es necesario hacer esta modificación.
Identifica dónde está el Attractor en la simulación. Cambia el color de este.
Observa que el Attractor tiene dos atributos this.dragging y this.rollover. Estos atributos no se modifican en el código, pero permitirían mover el attractor con el mouse y cambiar su color cuando el mouse está sobre él. ¿Cómo podrías modificar el código para que esto funcione? considera las funciones que ofrece p5.js para [interactuar con el mouse](https://p5js.org/reference/).

</details>

<details>
  <summary>Actividad 5 Coordenadas Polares</summary><br>

Explora otro sistema de coordenadas útil cuando se trabaja con ángulos. Se trata de las coordenadas polares.

Considera esta simulación de [coordenadas polares](https://editor.p5js.org/juanferfranco/sketches/fE5rCtDS1):

- Observa de nuevo esta parte del código ¿Cuál es la relación entre r y theta con las posiciones x y y? Puedes repasar entonces la definición de coordenadas polares y cómo se convierten a coordenadas cartesianas.

  - 

```js
function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  // Convert polar to cartesian
  let x = r * cos(theta);
  let y = r * sin(theta);
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, x, y);
  circle(x, y, 48);
  theta += 0.02;
}
```

Modifica la función `draw()`:

```js
 function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  let v = p5.Vector.fromAngle(theta);
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, x, y);
  circle(v.x, v.y, 48);
  theta += 0.02;
}
```

- ¿Qué ocurre? ¿Por qué?

  - 

Ahora realiza esta modificación:

```js
 function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  let v = p5.Vector.fromAngle(theta,r);
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, v.x, v.y);
  circle(v.x, v.y, 48);
  theta += 0.02;
}
```

- ¿Qué ocurre aquí? ¿Por qué?

  - 

</details>

<details>
  <summary>Actividad 6 Funciones Sinusoides</summary><br>

Repasa la función sinusoide [aquí](https://es.wikipedia.org/wiki/Sinusoide).

- Recuerda estos conceptos: velocidad angular, frecuencia, periodo, amplitud y fase.

  -

- Realiza una simulación en la que puedas modificar estos parámetros y observar cómo se comporta la función sinusoide.

  - 

Por ejemplo, te doy ideas, si juego solo con la fase, mira [este ejemplo](https://editor.p5js.org/juanferfranco/sketches/201gcBvjy).

</details>

<details>
  <summary>Actividad 7 Repaso de conceptos Unidades anteriores</summary><br>

Aplica conceptos de la unidades anteriores tomando como base [esta](https://editor.p5js.org/natureofcode/sketches/b3HpgJa6F) simulación. La idea es que la modifiques incluyendo un concepto de la unidad 1 (aleatoriedad, distinta a random) y la unidad 3 (fuerzas).

</details>

<details>
  <summary>Actividad 8 Ondas</summary><br>

Vas a observar [este](https://editor.p5js.org/natureofcode/sketches/CQ19Yw0iT) código que simula una onda.

El reto es que hagas que se esta onda se mueva como una ola.

</details>

<details>
  <summary>Actividad 9 Resortes</summary><br>

Modifica [esta](https://editor.p5js.org/natureofcode/sketches/HZOUeCe9p) simulación para crear un sistema de dos resortes conectados en serie.

</details>

<details>
  <summary>Actividad 10 Péndulos</summary><br>

Modifica [esta](https://editor.p5js.org/natureofcode/sketches/MQZWruTlD) simulación para crear un sistema de dos péndulos conectados en serie.

</details>

  
</details>

---

## Apply 

**Obra de arte generativa algorítmica interactiva en tiempo real**

Diseña e implementa una obra de arte generativa algorítmica interactiva en tiempo real en p5.js que cumpla con los siguientes requisitos:

- Selecciona uno de los conceptos con los que experimentaste en la fase de investigación y propón la obra alrededor de este.
- Incorpora en tu obra al menos un concepto de cada una de las unidades 1, 2 y 3.
- La obra debe ser interactiva en tiempo real. Puedes usar teclado, mouse, música, el micrófono, video, sensor o cualquier otro dispositivo de entrada.

### Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
>

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
>

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
>

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
>

### ¿Cómo resolviste la interacción?
> Tu respuesta aquí:
>

### Enlace a la obra en el editor de p5.js

[Aquí está mi obra](URL)

### Código de la obra 

``` js

```

### Captura de pantalla representativa








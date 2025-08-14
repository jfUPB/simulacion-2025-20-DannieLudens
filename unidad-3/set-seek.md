# Unidad 3

## 🔎 Fase: Set + Seek

<details>
  <summary>Actividad 01 – Inspiración: Alexander Calder</summary>

## Actividad 01 – Inspiración: Alexander Calder

Observé el video "The Artist as Inventor", centrado en el trabajo del artista Alexander Calder "Mariposa".

Calder es reconocido por sus esculturas móviles, que se mueven al interactuar con fuerzas como el viento y la gravedad. 

Sus obras combinan elementos visuales simples con un diseño preciso que permite un movimiento fluido y equilibrado.

<img width="300" src="https://github.com/user-attachments/assets/01778414-a63d-4ea2-9a25-11c64a4b36fe" />
<img width="370" src="https://github.com/user-attachments/assets/ff7475bb-c613-40f3-b066-2d1081d9c409" />


</details>

<details>
  <summary>Actividad 02 – Inspiración, reflexion y exploracion</summary>

## Actividad 02 – Inspiración, reflexion y exploracion

Exploré el proyecto Data Structure del estudio de diseño SOSO, el cual demuestra cómo las experiencias digitales pueden integrarse en el mundo físico mediante instalaciones interactivas.

Me imaginé instalaciones como las que aprendemos en la materia de mecanismos y prototipado donde las esculturas cinéticas pueden ser manipuladas por experiencias digitales, donde el color y el movimiento se modifican en tiempo real según datos o interacciones del usuario.

tambíen llegó recordé un juego de Mario kart que controla un kart fisico con una aplicacion y la sala de la casa se convierte en el circuito de carreras mientras se maneja de manera digital manifestandose en el mundo fisico

Recordé que la interacción como hemos estado acostumbrados a verla ha sido siempre de algo analogico hacia algo digital(como un teclado a un computador), y aunque en muchos casos la interacción puede ser bilateral(escanear, editar e imprimir), pensar en que desde lo digital se puede manipular elementos fisicos para dar vida a esas obras expresivas, amplia el panorama de las posibilidades y combinacion de disciplinas.

<img width="450" height="450" alt="image" src="https://github.com/user-attachments/assets/246b8901-b151-40b0-a951-89bd616842c2" />

<img width="236" height="419" alt="image" src="https://github.com/user-attachments/assets/9acd1ca6-449c-4134-8b1c-b017593de886" />



</details>

<details>
  <summary>Actividad 03 – Capítulo 2 de The Nature of Code, fuerzas y cómo aplicarlas</summary>

## Actividad 03 – Capítulo 2 de The Nature of Code, fuerzas y cómo aplicarlas

En esta fase trabajaremos el [Capítulo 2 de The Nature of Code](https://natureofcode.com/forces/), que corresponde a la Unidad 2 del curso. Este capítulo se enfoca en las fuerzas y en cómo aplicarlas dentro de simulaciones, basándose en las leyes de Newton.

Aquí profundizaremos en cómo modelar fuerzas como la gravedad, la fricción o el arrastre, y cómo estas afectan a los objetos en movimiento. El objetivo es entender cómo traducir las ecuaciones y principios físicos a código para crear comportamientos más realistas en nuestras simulaciones interactivas.

</details>

<details>
  <summary>Actividad 04 – Marco Motion 101</summary>

## Actividad 04 – Marco Motion 101

El marco Motion 101 es como la “receta básica” para mover un objeto en una simulación. Se basa en tres pasos:

1. **Calcular aceleración** → Aquí definimos cómo y hacia dónde se va a mover el objeto (puede ser constante, aleatoria, hacia el mouse, etc.).

2. **Actualizar velocidad** → Sumamos la aceleración a la velocidad actual, y limitamos la velocidad máxima para que no se dispare.

3. **Actualizar posición** → Sumamos la velocidad a la posición, lo que realmente hace que el objeto se desplace en pantalla.

En el código, esto se ve en el método `update()` este ciclo se repite en cada frame, creando un movimiento fluido que responde a la lógica de aceleración que hayamos definido:

<details>
  <summary>codigo</summary>

```js
let mover;

function setup() {
    createCanvas(640, 240);
    mover = new Mover();
}

function draw() {
    background(255);
    mover.show();
    mover.update();
    mover.checkEdges();
}

.
.
.

update() {

    // Aquí calculo la aceleración
    .
    .
    .
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topSpeed);
    this.position.add(this.velocity);
}
.
.
.
```
</details>

`this.velocity.add(this.acceleration)`; → La velocidad aumenta según la aceleración.

`this.velocity.limit(this.topSpeed)`; → Evitamos que la velocidad supere un máximo.

`this.position.add(this.velocity)`; → Movemos el objeto sumando la velocidad a la posición actual.

</details>

<details>
  <summary>Actividad 05 – Leyes de Newton y arte generativo</summary>

## Actividad 05 – Leyes de Newton y arte generativo

### Problema que veo en el planteamiento:


Si en el método `applyForce()` simplemente hacemos

```js
applyForce(force) {
  this.acceleration = force;
}
```
estamos sobrescribiendo la aceleración con cada fuerza nuevaen lugar de sumarla. Esto hace que solo la última fuerza aplicada en ese frame tenga efecto, ignorando todas las anteriores (ej: Si aplicamos primero el viento y luego la gravedad, la gravedad reemplaza al viento, y el objeto solo "recuerda" la última fuerza aplicada)

### Solución propuesta

La solución es sumar todas las fuerzas en cada frame y al final del `update()`, reiniciar la aceleración para evitar acumulación infinita de fuerzas

En lugar de reemplazar, debemos acumular todas las fuerzas que actúan en el frame actual:

```js
applyForce(force) {
  // Sumamos la fuerza a la aceleración actual
  this.acceleration.add(force);
}
```

Después, en `update()`:

1. Usamos la aceleración acumulada para actualizar la velocidad y la posición

2. Reiniciamos la aceleración a (0,0) para el siguiente frame, ya que en el nuevo frame las fuerzas pueden ser distintas

<details>
  <summary>asi seria la implementacion en p5js (Click aqui)</summary>

```js
class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2); // Posición inicial
    this.velocity = createVector(0, 0); // Velocidad inicial
    this.acceleration = createVector(0, 0); // Aceleración inicial
    this.mass = 1; // Masa (puede cambiarse si se quiere más realismo por el momento dejemosla en masa 1)
  }

  // Método para aplicar una fuerza
  applyForce(force) {
    // Aceleración = Fuerza / masa
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  // Actualizar movimiento
  update() {
    this.velocity.add(this.acceleration); // Sumar aceleración a la velocidad
    this.position.add(this.velocity);     // Sumar velocidad a la posición
    this.acceleration.mult(0);            // Reiniciar aceleración para el siguiente frame
  }

  // Mostrar el objeto
  show() {
    stroke(0);
    fill(175);
    ellipse(this.position.x, this.position.y, 20, 20);
  }
}

let mover;

function setup() {
  createCanvas(640, 360);
  mover = new Mover();
}

function draw() {
  background(255);

  let wind = createVector(0.1, 0); // Fuerza hacia la derecha
  let gravity = createVector(0, 0.2); // Fuerza hacia abajo

  mover.applyForce(wind);
  mover.applyForce(gravity);

  mover.update();
  mover.show();
}

```
</details>

[Link a sketch p5js](https://editor.p5js.org/DanielZafiro/sketches/My9QaTRty)


<img src="https://github.com/user-attachments/assets/a4a2d360-6062-4a5b-93aa-247c03bb8b76" width="400">

</details>

<details>
  <summary>Actividad 06 – titulo</summary>

## Actividad 06 – titulo

texto

</details>

<details>
  <summary>Actividad 07 – titulo</summary>

## Actividad 07 – titulo

texto

</details>

<details>
  <summary>Actividad 08 – titulo</summary>

## Actividad 08 – titulo

texto

</details>

<details>
  <summary>Actividad 09 – titulo</summary>

## Actividad 09 – titulo

texto

</details>


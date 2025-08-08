# Unidad 2


## 🛠 Fase: Apply


### Obra generativa interactiva: Campo de Luciérnagas

#### Descripción del concepto

Inspirada en la bioluminiscencia de las luciérnagas, esta obra genera en tiempo real un ecosistema de partículas que se comportan como insectos nocturnos voladores. Cada luciérnaga se mueve con un patrón fluido e impredecible, titila orgánicamente y reacciona a la presencia del mouse, ya sea acercándose con curiosidad o huyendo con nerviosismo. Esta simulación busca transmitir la sensación de estar inmerso en un bosque vivo y dinámico, resaltando la belleza de los movimientos colectivos y la interacción sutil con el entorno.

#### Aplicación de conceptos de la Unidad 2

##### Vectores

La obra usa objetos `p5.Vector` para representar la posición, velocidad y aceleración de cada luciérnaga. Estos tres vectores determinan su movimiento mediante la fórmula base del modelo de Motion 101:

```javascript
velocity.add(acceleration);
position.add(velocity);
```

Esto permite separar claramente el control de la dirección del movimiento (aceleración) y la inercia (velocidad), logrando un desplazamiento más natural.

##### Aceleración aleatoria (con Perlin noise)

Cada luciérnaga genera una aceleración suave y continua basada en Perlin noise. Esto evita el movimiento errático de una aceleración puramente aleatoria y produce trayectorias más orgánicas. Se calcula así:

```javascript
acceleration.x += map(noise(offset.x), 0, 1, -0.02, 0.02);
acceleration.y += map(noise(offset.y), 0, 1, -0.02, 0.02);
```

Además, si la luciérnaga está cerca del mouse, se le agrega otra aceleración que depende de su comportamiento: acercarse o alejarse del cursor.

##### Control de velocidad

Las luciérnagas tienen un límite de velocidad bajo (`velocity.limit(0.5)`) para que el movimiento sea lento, flotante y realista. Esto es fundamental para que la experiencia visual sea relajante y no caótica.

##### Interpolación (lerp) para simular titileo

El efecto de parpadeo se logra interpolando el brillo (`brightness`) entre 0 y 255 con `lerp()`, según un sistema de estados que alterna entre "encender", "apagar" y "pausa":

```javascript
brightness = lerp(brightness, 255, 0.05); // encendido progresivo
brightness = lerp(brightness, 0, 0.05);   // apagado suave
```

Este parpadeo no es sincronizado entre las luciérnagas, ya que cada una tiene su temporizador interno, reforzando la ilusión de un sistema autónomo y natural.

#### Interacción

El mouse modifica el comportamiento de las luciérnagas:

- Algunas se acercan suavemente si están a menos de 120 píxeles.
- Otras se alejan simulando una reacción defensiva.
- Cuando el mouse se mueve, se generan agrupaciones o dispersiones dinámicas que enriquecen la composición.


<details>
    <summary>Sketch.js</summary>

```js
let luciernagas = [];

function setup() {
  createCanvas(600, 400);
  noStroke();
  for (let i = 0; i < 60; i++) {
    luciernagas.push(new Luciernaga());
  }
}

function draw() {
  background(10, 10, 30); // Fondo noche
  for (let l of luciernagas) {
    l.update();
    l.show();
  }
}

class Luciernaga {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = p5.Vector.random2D().mult(0.2);
    this.acceleration = createVector(0, 0);
    this.noiseOffset = createVector(random(1000), random(1000), random(1000));
    this.size = random(4, 10);

    this.brightness = 0;
    this.state = "pausa"; // puede ser: encendiendo, apagando, pausa
    this.timer = random(30, 120); // duración de cada estado
    this.behavior = random() < 0.5 ? "huir" : "acercarse";
  }

  update() {
    this.acceleration.mult(0); // Reiniciar aceleración

    // Movimiento con Perlin noise
    let noiseScale = 0.01;
    this.acceleration.x += map(noise(this.noiseOffset.x), 0, 1, -0.02, 0.02);
    this.acceleration.y += map(noise(this.noiseOffset.y), 0, 1, -0.02, 0.02);
    this.noiseOffset.add(0.01, 0.01, 0.01);

    // Comportamiento con el mouse
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, this.position);
    let d = dir.mag();

    if (d < 120) {
      dir.normalize();
      let force = map(d, 0, 120, 0.2, 0); // Menos fuerza cuando están más lejos
      if (this.behavior === "huir") dir.mult(-force);
      else dir.mult(force);
      this.acceleration.add(dir);
    }

    // Motion 101
    this.velocity.add(this.acceleration);
    this.velocity.limit(0.5);
    this.position.add(this.velocity);

    // Rebote en bordes
    if (this.position.x < 0 || this.position.x > width) this.velocity.x *= -1;
    if (this.position.y < 0 || this.position.y > height) this.velocity.y *= -1;

    // --- Parpadeo con estados ---
    this.timer--;

    if (this.state === "encendiendo") {
      this.brightness = lerp(this.brightness, 255, 0.05);
      if (this.timer <= 0) {
        this.state = "apagando";
        this.timer = random(30, 100);
      }
    } else if (this.state === "apagando") {
      this.brightness = lerp(this.brightness, 0, 0.05);
      if (this.timer <= 0) {
        this.state = "pausa";
        this.timer = random(60, 180);
      }
    } else if (this.state === "pausa") {
      this.brightness = 0;
      if (this.timer <= 0) {
        this.state = "encendiendo";
        this.timer = random(30, 100);
      }
    }
  }

  show() {
    // Solo se dibujan si hay algo de brillo visible
    if (this.brightness > 1) {
      fill(this.brightness, this.brightness, 0, 200);
      ellipse(this.position.x, this.position.y, this.size);
    }
  }
}

```
</detials>

https://editor.p5js.org/DanielZafiro/sketches/mki7fdtaY

<img src="" width="">

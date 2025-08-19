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
  <summary>Actividad 06 – La fuerza neta debe ser acumulativa</summary>

## Actividad 06 – La fuerza neta debe ser acumulativa

En esta parte la lógica es clave para que el sistema de fuerzas funcione como en el mundo real

### 1. ¿Por qué es necesario multiplicar la aceleración por cero en cada frame?

Porque la aceleración no es una propiedad permanente del objeto, sino el resultado de las fuerzas que actúan solamente en ese instante

Si no la reiniciamos, la aceleración acumulada del frame anterior seguiría sumándose en el siguiente, y el objeto se movería como si las fuerzas fueran permanentes aunque ya no existieran. Esto haría que el objeto pareciera “acelerarse solo” incluso cuando ya no le aplicamos fuerzas

<img width="136" height="78" alt="image" src="https://github.com/user-attachments/assets/697c38a0-a0ba-4ff1-a89d-cfb24b86b59b" />

La suma de fuerzas se recalcula cada instante. Si en el próximo instante no hay fuerzas, la aceleración debería ser cero

### 2. ¿Por qué se multiplica por cero justo al final de update()?

Porque `update()` es el paso donde usamos la aceleración acumulada para cambiar la velocidad y posición.

1. Primero: aplicamos todas las fuerzas con `applyForce()` → se suman en `this.acceleration`

2. Luego: en `update()` esa aceleración se usa para modificar la velocidad

3. Por último: la aceleración se reinicia (`mult(0)`) para que en el siguiente frame empiece limpio, listo para calcular nuevas fuerzas

> [!TIP]
> Analogía:
> Imaginar que cada frame es como un empujón en una patineta:
> 
> - Durante un frame, se acumula todos los empujones que recibo.
> - Al final, se usa esa fuerza acumulada para moverme.
> - Luego reseteo la fuerza a cero para esperar nuevos empujones en el siguiente momento.

ejemplo de lo que pasaria si no resetearamos la fuerza:

[link a sketch](https://editor.p5js.org/DanielZafiro/sketches/sahf6mdm3)

<img src="https://github.com/user-attachments/assets/d5b5c5ee-6222-4620-b56d-680a55a75833" width="600">


</details>

<details>
  <summary>Actividad 07 – En mi mundo los pixeles si tienen masa</summary>

## Actividad 07 – En mi mundo los pixeles si tienen masa

Lo que tenemos 

```js
applyForce(force) {
    // Asume que la masa es 10
    force.div(10);
    this.acceleration.add(force);
}
```
y llamamos 

```js
mover.applyForce(wind);
mover.applyForce(gravity);
```

### Detectamos el "problema" 

`wind` y `gravity` son objetos `p5.Vector` y **en JavaScript, los objetos se pasan por referencia, no por valor.** Esto significa que si modificamos `force` dentro de `applyForce`, estamos modificando el vector original que se pasó.

#### ¿Qué pasa aquí?

`force.div(10)` divide el vector original por 10.

Entonces, si pasaste `wind` y luego quieres usar `wind` otra vez en otro lado, ya no tiene los valores originales, sino que fue alterado.

Esto es un efecto no deseado cuando aplicas varias fuerzas en frames consecutivos o cuando reutilizas vectores.

#### Como arreglarlo

La solución es no modificar el vector original, sino trabajar con una copia En p5.js usamos `.copy()`

crear una copia de la fuerza antes de dividir por la masa y sumarla a la aceleración

```js
applyForce(force) {
    let f = force.copy();  // Crea una copia para no alterar el original
    f.div(this.mass);      // ahora sí dividimos la copia
    this.acceleration.add(f);
}
```

Ahora `wind` y `gravity` no se alteran y podemos aplicarlas tantas veces como queramos sin efectos colaterales



</details>

<details>
  <summary>Actividad 08 – Paso por valor y paso por referencia
</summary>

## Actividad 08 – Paso por valor y paso por referencia

### Concepto clave: VALOR vs REFERENCIA

Cuando por valor y cuando por referencia

| Concepto       | Qué significa                             | Ejemplo en p5.Vector   |
| -------------- | ----------------------------------------- | ---------------------- |
| **Valor**      | Se crea un nuevo objeto, independiente    | `this.velocity.copy()` |
| **Referencia** | Se trabaja sobre el mismo objeto original | `this.velocity`        |

* **Paso por valor** → seguro para cálculos temporales, no altera el original.
* **Paso por referencia** → útil para manipular el objeto real, pero puede causar errores si no se controla.

```js
let friction = this.velocity.copy();
let friction = this.velocity;
```

### Diferencia entre paso por valor y paso por referencia

1. `let friction = this.velocity.copy();`

- Se crea una copia independiente del vector `this.velocity`

- Cambiar `friction` no afecta a `this.velocity`

- Esto es **paso por valor**, porque estamos trabajando con un nuevo objeto que contiene los mismos datos.

2. `let friction = this.velocity;`

- `friction` apunta al mismo objeto que `this.velocity`

- Cambiar `friction` también cambia `this.velocity`

- Esto es **paso por referencia**, porque ambos nombres referencian el mismo objeto en memoria.

### Qué podría salir mal con `let friction = this.velocity;`

* Si modificamos `friction` (por ejemplo `friction.mult(0.9)` para simular fricción), también se **modifica `this.velocity`**, lo que puede generar **efectos inesperados** en el movimiento del objeto.
* Esto rompe la lógica física, porque la fricción debería afectar solo la aceleración o la fuerza aplicada, no alterar directamente el vector de velocidad original antes de sumarlo a la posición.


</details>

<details>
  <summary>Actividad 09 – Modelando fuerzas</summary>

## Actividad 09 – Modelando fuerzas

### 🌌 Fricción


<img src="https://github.com/user-attachments/assets/c4b0256e-99d4-4290-93bd-b468e4efd6b3" width="400">


**Concepto obra interactiva**

Un círculo se mueve por la pantalla con una velocidad inicial y poco a poco se va deteniendo debido a la fricción. El usuario puede **arrastrar con el mouse** sobre el círculo para empujarlo en distintas direcciones, pero siempre se observa cómo la fricción reduce su movimiento con el tiempo.

**Cómo modelé la fuerza**

La fricción se modeló como un **vector opuesto a la velocidad**:

1. Se copió la velocidad (`this.velocity.copy()`)
2. Se invirtió su dirección (`mult(-1)`)
3. Se normalizó para dejar solo la dirección
4. Se multiplicó por un **coeficiente de fricción** ($\mu$)
5. Finalmente, se aplicó como fuerza con `applyForce(friction)`

<img width="139" height="40" alt="image" src="https://github.com/user-attachments/assets/77e3b2a1-bd6b-4622-8a9a-68560c84e458" />

donde 𝑣^ es el vector velocidad normalizado.

**Relación conceptual con la obra generativa**

La fricción le da al movimiento un **comportamiento natural**, ya que ningún objeto en el mundo real se mueve indefinidamente sin que algo lo frene. La obra permite experimentar cómo el círculo pierde velocidad hasta detenerse, y cómo al arrastrarlo con el mouse siempre termina cediendo ante la fricción.

<details>
  <summary>sketch.js</summary>

```js
let mover;

function setup() {
  createCanvas(640, 360);
  mover = new Mover();
}

function draw() {
  background(255);

  mover.applyFriction(0.1); // aplicamos fricción
  mover.update();
  mover.show();
  
  fill(50);
  textSize(20);
  text(`Arrastra el circulo`, 10, 30);
  
}


function mouseDragged() {
  let force = createVector(mouseX - pmouseX, mouseY - pmouseY);
  force.setMag(0.14);
  mover.applyForce(force);
}

class Mover {
  constructor() {
    this.position = createVector(width/2, height/2);
    this.velocity = createVector(random(-5,5), random(-5,5));
    this.acceleration = createVector(0,0);
    this.mass = 1;
  }

  applyForce(force) {
    let f = force.copy().div(this.mass);
    this.acceleration.add(f);
  }

  applyFriction(mu) {
    let friction = this.velocity.copy(); // paso por valor (copia) para no alterar la velocidad original
    friction.mult(-1);                    // dirección contraria al movimiento
    friction.normalize();                 // solo queremos la dirección
    friction.mult(mu);                    // aplicamos el coeficiente de fricción
    this.applyForce(friction);            // sumamos como fuerza
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    fill(175);
    ellipse(this.position.x, this.position.y, 20, 20);
  }
}
```
</details>

[sketch laberinto ObraGen](https://editor.p5js.org/DanielZafiro/sketches/FxUcm0TBH)


### 🌌 Resistencia del aire y fluidos

<img src="https://github.com/user-attachments/assets/67e78b0c-d0ad-46eb-916d-adc54fde0731" width="400">

**Idea de la obra**

En esta obra generativa interactiva los objetos (círculos) caen bajo el efecto de la gravedad, pero al mismo tiempo enfrentan una fuerza de resistencia que depende de su velocidad y de su masa. El usuario puede hacer clic en la pantalla para reiniciar la simulación y generar tres nuevos círculos con masas aleatorias, lo que hace que cada ejecución sea diferente

**Cómo modelé la fuerza**

La resistencia del aire o fluido se modela con la siguiente fórmula

<img width="150" height="46" alt="image" src="https://github.com/user-attachments/assets/91fc0d56-f992-48d5-b247-7e2795be3308" />

Donde:

𝑐 es el coeficiente de resistencia del fluido.

𝑣 es la magnitud de la velocidad.

𝑣^ es la dirección de la velocidad normalizada.

El signo negativo indica que la fuerza siempre se opone al movimiento.


* La fuerza siempre va en dirección contraria al movimiento.
* Es proporcional al cuadrado de la velocidad del objeto.
* Multiplico el vector velocidad por $-1$, lo normalizo y luego lo escalo según la magnitud de la velocidad al cuadrado y un coeficiente $c$ que representa la densidad del fluido.
* Finalmente aplico esa fuerza al objeto usando su masa.

**Relación conceptual con la obra**

El concepto de **resistencia de fluidos** se traduce aquí en la sensación de que los círculos caen con diferentes masa y por ende su velocidad. Los más ligeros se frenan más rápido, mientras que los más pesados atraviesan con más fuerza el "aire". Esto genera una diversidad de trayectorias como en fenómenos naturales como hojas cayendo, burbujas moviéndose en el agua o gotas de lluvia, etc.

<details>
  <summary>sketch.js</summary>
  
```js
let movers = [];

function setup() {
  createCanvas(640, 360);
  resetMovers();
}

function draw() {
  background(255);

  for (let mover of movers) {
    // fuerza de gravedad proporcional a la masa
    let gravity = createVector(0, 0.2 * mover.mass);
    mover.applyForce(gravity);

    // resistencia del fluido
    mover.applyDrag(0.05);

    mover.update();
    mover.show();
    mover.edges();
  }
}

// cada click reinicia con 3 círculos de masa random
function mousePressed() {
  resetMovers();
}

function resetMovers() {
  movers = [];
  for (let i = 0; i < 3; i++) {
    movers.push(new Mover(random(0.5, 4), random(width), 0));
  }
}

class Mover {
  constructor(mass, x, y) {
    this.mass = mass;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = force.copy().div(this.mass);
    this.acceleration.add(f);
  }

  applyDrag(c) {
    let drag = this.velocity.copy();
    drag.mult(-1);
    let speedSq = this.velocity.magSq();
    drag.normalize();
    drag.mult(c * speedSq);
    this.applyForce(drag);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    fill(175, 150);
    ellipse(this.position.x, this.position.y, this.mass * 16, this.mass * 16);
  }

  edges() {
    if (this.position.y > height - this.mass * 8) {
      this.position.y = height - this.mass * 8;
      this.velocity.y *= -0.5;
    }
  }
}
```
</details>

[sketch pecera objetos que caen con diferentes masas](https://editor.p5js.org/DanielZafiro/sketches/7KyprfAVg)

### 🌌 Atraccion gravitacional

<img src="https://github.com/user-attachments/assets/9bc5e538-dd47-48e2-98ae-ddf550e344aa" width="400">

* **Modelado matemático:**

  Se basa en la Ley de Gravitación Universal de Newton:

  <img width="120" height="54" alt="image" src="https://github.com/user-attachments/assets/229a8d23-a482-4268-9ceb-75b387d97637" />


  donde $G$ es una constante gravitacional (que escalamos a un valor útil para la simulación), $m_1$ y $m_2$ son las masas de los cuerpos y $d$ es la distancia entre ellos.
  La dirección de la fuerza es desde el objeto hacia el planeta.

**Relación con la obra:**

  * El círculo central representa a **Júpiter**, un planeta inmenso.
  * Los cuerpos pequeños son asteroides o satélites que al hacer clic aparecen en diferentes posiciones y masas.
  * Se mueven afectados únicamente por la gravedad central, produciendo trayectorias curvas y a veces órbitas.
  * Es interactiva porque con **cada clic** se generan nuevos cuerpos que son atraídos por Júpiter.


<details>
  <summary>sketch.js</summary>
  
```js
let jupiter;
let movers = [];

function setup() {
  createCanvas(640, 360);
  jupiter = new Mover(width/2, height/2, 20, true); // planeta grande en el centro
}

function draw() {
  background(0);

  // Dibujar planeta
  jupiter.show(color(255, 204, 0));

  // Actualizar y mostrar los asteroides
  for (let mover of movers) {
    let force = jupiter.attract(mover); // fuerza gravitacional
    mover.applyForce(force);
    mover.update();
    mover.show(color(175));
  }

  fill(255);
  textSize(16);
  text("Click para lanzar asteroides atraídos por Júpiter", 10, 20);
}

function mousePressed() {
  // Crear 1 nuevo asteroides en posiciones random
  for (let i = 0; i < 1; i++) {
    let x = random(width);
    let y = random(height);
    let mass = random(2, 8); // masas variadas
    movers.push(new Mover(x, y, mass, false));
  }
}

class Mover {
  constructor(x, y, m, isFixed) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D().mult(random(1, 3));
    this.acceleration = createVector(0, 0);
    this.mass = m;
    this.isFixed = isFixed; // si es Júpiter no se mueve
  }

  applyForce(force) {
    if (!this.isFixed) {
      let f = force.copy().div(this.mass);
      this.acceleration.add(f);
    }
  }

  update() {
    if (!this.isFixed) {
      this.velocity.add(this.acceleration);
      this.position.add(this.velocity);
      this.acceleration.mult(0);
    }
  }

  attract(mover) {
    let force = p5.Vector.sub(this.position, mover.position); // dirección
    let distance = constrain(force.mag(), 5, 25); // evitar división por cero
    force.normalize();
    let G = 1;
    let strength = (G * this.mass * mover.mass) / (distance * distance);
    force.mult(strength);
    return force;
  }

  show(c) {
    noStroke();
    fill(c);
    ellipse(this.position.x, this.position.y, this.mass * 4);
  }
}
```
</details>

[sketch jupiter](https://editor.p5js.org/DanielZafiro/sketches/I1sG-tunu)

</details>









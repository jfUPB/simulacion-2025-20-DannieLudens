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
  <summary>Actividad 09 – titulo</summary>

## Actividad 09 – titulo

[sketch laberinto ObraGen](https://editor.p5js.org/DanielZafiro/sketches/FxUcm0TBH)

[sketch pecera objetos que caen con diferentes masas]()

[sketch jupiter]()

</details>





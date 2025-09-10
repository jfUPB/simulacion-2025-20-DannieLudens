# Unidad 4

## 🔎 Fase: Set + Seek

**Introducción** 📜

En las unidades anteriores has explorado el movimiento con el marco motion 101 manipulando la posición de los elementos gráficos. En esta unidad explorarás el movimiento angular u oscilatorio.

**Set: ¿Qué aprenderás en esta unidad?** 💡

- Comprender los conceptos básicos del movimiento angular y oscilatorio.
- Explorar cómo se pueden aplicar estos conceptos en la creación de obras generativas interactivas en tiempo real.

---

**Seek: Investigación** 🔎

Exploración y desarrollo de la unidad 4

### Actividades de la unidad 4

**Proceso de desarrollo y aprendizaje**

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

  - La interacción consiste en que al presionar cualquier tecla se va a aumentar en 0.1 el angulo de rotacion de las figuras dibujadas desde el centro:
 
    <img src="https://github.com/user-attachments/assets/2ddb8f67-da32-42de-a3b6-baaee69484a6" width="200">

- Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?

  - Esto es porque se esta usando `translate(width/2, height/2)`. Normalmente el origen `(0,0)` está en la esquina superior izquierda del lienzo.
   
    Si no moviéramos el origen, la rotación ocurriría alrededor de esa esquina, lo cual daría un efecto raro (como que los objetos dieran vueltas en la esquina superior izquierda del canvas).

    Con `translate(width/2, height/2)`, el origen del sistema de coordenadas se coloca en el centro del canvas establecido(640x240). Así, la rotación ocurre alrededor del centro, lo que visualmente se siente como un péndulo o un objeto que gira desde el medio.
  
- Cuál es la relación entre el sistema de coordenadas y la función `rotate()`.

  - Ese `angle` se pasa a la función `rotate(angle)`, la cual rota todo el sistema de coordenadas actual, no los objetos por separado.
    
    <img src="https://github.com/user-attachments/assets/e976268b-d65f-4c9f-8611-8952033dde5f" width="300">

    Entonces todo los objetos que se dibujen despues de `rotate(angle)` van a rotar, como el cuadrado se dibuja antes de la funcion no rota.

- Nota esta parte del código:

```js
  line(-50, 0, 50, 0);
  stroke(0);
  strokeWeight(2);
  fill(127);
  circle(50, 0, 16);
  circle(-50, 0, 16);
```

Observa que al dibujar los elementos gráficos parece que se está dibujando en la posición `(0, 0)` del sistema de coordenadas. 

- ¿Por qué crees que se hace esto?

  - Si no existiera la transformación (`translate` y `rotate`), se estarían dibujando a lo largo del eje X con respecto al origen `(0,0)` en la esquina superior izquierda.
 
    Pero en realidad, antes de dibujarlos, se movio el origen con

    ```js
    translate(width/2, height/2);
    ```
    el cual lo posiciona en la mitad del ancho y alto del canvas. Por eso los objetos parecen estar anclados al centro: porque efectivamente sus coordenadas están medidas desde ese nuevo sistema.

- ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?

  - En cada `draw()` se hace exactamente el mismo código de dibujo (línea y círculos).

    Lo que cambia es la transformación aplicada antes de dibujar:

    ```js
    rotate(angle);
    ```
    Como `angle` va aumentando cada vez que se presiona una tecla, el sistema de coordenadas está rotado un poco más en cada frame.
    
    Eso hace que aunque “dibujes lo mismo en (50,0) y (-50,0)”, esas posiciones ya no apuntan en la misma dirección que antes, porque los ejes X-Y están girados.

Ahora analiza una simulación que muestra cómo puedes hacer para que los elementos gráficos de la simulación [apunten en la dirección del movimiento](https://editor.p5js.org/natureofcode/sketches/bZqHGYbRQ).

- Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?

  - El marco Motion 101 en este ejemplo consiste en actualizar la posición con la velocidad y la velocidad con la aceleración.

    La aceleración apunta hacia el mouse, de modo que el objeto se ve atraído hacia él, y el ángulo de la velocidad se usa para orientar el objeto en la dirección en la que realmente se mueve

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

  - La función `heading()` es un método que se aplica a los vectores (en este caso, al vector `this.velocity`). Su propósito es calcular y devolver el ángulo de rotación de ese vector en 2D. Este ángulo se mide en relación con el eje X positivo.
    
    En el contexto de la simulación, `this.velocity.heading()` da la dirección en la que el objeto se está moviendo

- ¿Qué hace la función push() y pop()? Realiza algunos experimentos para entender su funcionamiento.

  - Las funciones `push()` y `pop()` trabajan en pareja para guardar y restaurar el "estado" del lienzo. 
    
    Piensa en `push()` como "guardar una copia de la configuración actual de dibujo" (colores, grosores de línea, transformaciones como translate y rotate). Después de llamar a `push()`, puedes hacer todos los cambios que quieras.

    Cuando llamas a `pop()`, el lienzo "recuerda" y vuelve a la configuración que guardaste con `push()`, descartando todos los cambios que hiciste en medio.

    ```js
      rect(-70, 50, 40, 40); // Cuadrado normal

      push(); // Guardamos el estado actual
      fill(255, 0, 0); // Cambiamos el color a rojo
      translate(10, 50); // Nos movemos a una nueva posición
      rotate(PI / 4); // Rotamos 45 grados
      rect(0, 0, 40, 40); // Dibujamos el cuadrado del centro (en el nuevo origen)
      pop(); // Restauramos el estado original
    
      rect(90, 50, 40, 40); // Este cuadrado no está rotado ni es rojo
    ```

    <img width="300" src="https://github.com/user-attachments/assets/c262a4fd-9701-48c1-8fb0-bbf0755b5848" />

- ¿Qué hace `rectMode(CENTER)`? Realiza algunos experimentos para entender su funcionamiento.

  - `rectMode(CENTER)` cambia la forma en que p5.js interpreta las coordenadas que se le da a la función `rect()`.

    Por defecto, los dos primeros parámetros de `rect(x, y, ancho, alto)` son la esquina superior izquierda del rectángulo.

    Cuando usamos `rectMode(CENTER)`, los dos primeros parámetros (`x` e `y`) se convierten en el centro del rectángulo. Y la razon es porque por defecto el rect se dibuja desde la esquina hacia abajo y la derecha
    
    [Experimento en p5js sketch:](https://editor.p5js.org/DanielZafiro/sketches/TVJoJag0h) y [Video de referencia](https://youtu.be/F7iRdN50jf8)

    <img width="500" src="https://github.com/user-attachments/assets/993af037-252f-4506-9c29-e99809a9ed48" />



- ¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.

  - La relación es directa y muy importante para lograr que un objeto "mire" hacia donde se mueve.

    El **vector de velocidad** tiene dos componentes: una magnitud (qué tan rápido se mueve) y una dirección (hacia dónde se mueve). La función `this.velocity.heading()` extrae esa **dirección** y la convierte en un **ángulo de rotación**.

</details>

<details>
  <summary>Actividad 3 Practicar un poco</summary><br>

Ahora es momento de practicar los conceptos anterior. Crea una simulación de un vehículo que puedas conducir por la pantalla utilizando las teclas de flecha: la flecha izquierda acelera el vehículo hacia la izquierda, y la flecha derecha acelera hacia la derecha. El vehículo tendrá forma triangular y debe apuntar en la dirección en la que se está moviendo actualmente.

---

<img width="700" src="https://github.com/user-attachments/assets/41ffa3a3-be8a-4230-ad5a-1a7df99303be">

Esta simulación es una aplicación directa de los conceptos de la unidad

<details>
  <summary>Experimento Sketch.js Simulacion de Vehiculo</summary><br>

1. **Marco Motion 101**: La clase `Vehicle` contiene los tres vectores clave: `position`, `velocity` y `acceleration`.

2. **Fuerzas**: Las teclas de flecha no modifican directamente la posición ni la velocidad. En su lugar, `applyForce()` añade un vector a la `acceleration`. Este es un concepto más realista de la física: las fuerzas causan aceleración. Al reiniciar la aceleración (`this.acceleration.mult(0)`) en cada fotograma, el vehículo solo acelera si se mantienes una tecla presionada.

3. **Orientación y Rotación**: En el método `display()`.

- `let angle = this.velocity.heading() + PI / 2;` es la línea más importante. Extraemos la dirección del movimiento (`this.velocity.heading()`) para saber hacia dónde rotar.

- Usamos `translate(this.position.x, this.position.y)` para mover el origen del sistema de coordenadas a la ubicación del vehículo.

- `rotate(angle)` alinea el lienzo con la dirección del movimiento.

- Finalmente, dibujamos un `triangle()` simple en `(0,0)`. Como todo el sistema de coordenadas ya está movido y rotado, el triángulo aparece en el lugar correcto y apuntando en la dirección correcta. El uso de `push()` y `pop()` asegura que estas transformaciones solo afecten al dibujo de nuestro vehículo.

```js
let vehicle;

function setup() {
  createCanvas(640, 360);
  // Creamos una nueva instancia de la clase Vehicle y la guardamos en la variable
  vehicle = new Vehicle(width / 2, height / 2);
}

function draw() {
  background(220,50);

  // Verificamos si las teclas de flecha están siendo presionadas
  if (keyIsDown(LEFT_ARROW)) {
    let force = createVector(-0.1, 0); // vector de fuerza que apunta a la izquierda
    vehicle.applyForce(force);
  }

  if (keyIsDown(RIGHT_ARROW)) {
    let force = createVector(0.1, 0); // vector de fuerza que apunta a la derecha
    vehicle.applyForce(force);
  }

  vehicle.update(); // lógica del vehículo (movimiento)
  vehicle.checkEdges(); // aparezca por el otro lado si se sale de la pantalla
  vehicle.display(); // Dibujamos el vehículo en su nueva posición y con la rotación correcta

  // Para mostrar la velocidad actual
  // 1. Calculamos la magnitud del vector de velocidad.
  let speedMeter = vehicle.velocity.mag();
  
  // 2. Preparamos el estilo del texto
  fill(0);           // Color negro para el texto
  textSize(16);      // Un tamaño de letra legible
  noStroke();        // El texto se ve mejor sin borde
  
  // 3. Dibujamos el texto y toFixed(2) para redondear el valor a 2 decimales y que no ocupe toda la pantalla.
  text("Velocidad: " + speedMeter.toFixed(2), 260, 150);
}

// --- Clase Vehicle ---
class Vehicle {
  constructor(x, y) {
    // El marco Motion 101: Posición, Velocidad y Aceleración
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    
    // Propiedades adicionales para controlar el comportamiento
    this.r = 6; // Tamaño del vehículo para el triángulo
    this.topspeed = 4; // Límite de velocidad
  }

  // Método para aplicar una fuerza (como la de las teclas)
  applyForce(force) {
    // La fuerza se suma a la aceleración (F=ma, si m=1, F=a)
    this.acceleration.add(force);
  }

  // Método principal que actualiza el estado del vehículo en cada fotograma
  update() {
    // La velocidad cambia por la aceleración
    this.velocity.add(this.acceleration);
    // Limitamos la velocidad para que no vaya infinitamente rápido
    this.velocity.limit(this.topspeed);
    // La posición cambia por la velocidad
    this.position.add(this.velocity);
    // Reiniciamos la aceleración a 0 en cada ciclo para que la fuerza no se acumule
    this.acceleration.mult(0);
  }

  // Método para dibujar el vehículo en el lienzo
  display() {
    // Aquí está la magia que conecta el movimiento con la rotación:
    // 1. Obtenemos el ángulo de la dirección del vector de velocidad.
    // 2. Sumamos PI/2 (90 grados) porque nuestro triángulo lo dibujaremos
    //    apuntando hacia arriba, y queremos que esa sea la dirección "cero".
    let angle = this.velocity.heading() + PI / 2;

    push(); // Guardamos el estado del lienzo
    translate(this.position.x, this.position.y); // 1. Nos movemos al centro del vehículo
    rotate(angle); // 2. Rotamos el lienzo entero a la dirección del movimiento

    // 3. Dibujamos el triángulo en el origen (0,0) del nuevo sistema de coordenadas
    fill(127);
    stroke(0);
    strokeWeight(1);
    // Los puntos del triángulo están definidos relativos a su centro (0,0)
    // - (0, -r*2) es la punta superior
    // - (-r, r) es la esquina inferior izquierda
    // - (r, r) es la esquina inferior derecha
    triangle(0, -this.r * 2, -this.r, this.r, this.r, this.r);

    pop(); // Restauramos el estado del lienzo para no afectar a otros dibujos
  }

  // Método para que el vehículo "envuelva" la pantalla
  checkEdges() {
    if (this.position.x > width + this.r) {
      this.position.x = -this.r;
    } else if (this.position.x < -this.r) {
      this.position.x = width + this.r;
    }

    if (this.position.y > height + this.r) {
      this.position.y = -this.r;
    } else if (this.position.y < -this.r) {
      this.position.y = height + this.r;
    }
  }
}
```
</details>

---

<img width="500" src="https://github.com/user-attachments/assets/4566ae4d-ba36-4fe5-9fef-c8247260d90c">

La diferencia fundamental es que ahora la orientación del vehículo y su dirección de movimiento no son necesariamente la misma cosa.

En el modelo anterior, el vehículo siempre apuntaba en la dirección en que se movía. Ahora, se puede girar el vehículo y luego decidir si se quiere acelerar en esa nueva dirección.

<details>
  <summary>Experimento apartir del ejercicio 3.6 del libro guia</summary><br>

1.  **Separación de Ángulo y Velocidad**: El cambio más importante es la adición de la propiedad `this.angle` en la clase `Vehicle`. Ahora, el vehículo tiene una orientación propia que no depende de hacia dónde se está moviendo.

      * En el método `display()`, la línea `rotate()` ahora usa `this.angle` en lugar de `this.velocity.heading()`. Esto significa que el triángulo apunta hacia donde tú le dices con las flechas.

2.  **Nuevos Controles**:

      * **Giro (`turn`)**: Las flechas izquierda y derecha ya no aplican una fuerza lateral. En su lugar, llaman al nuevo método `vehicle.turn()`, que simplemente suma o resta un pequeño valor al `this.angle`, haciendo que el vehículo gire sobre su propio eje.
      * **Empuje (`thrust`)**: La barra espaciadora llama al método `vehicle.thrust()`. Este método es clave:
          * `p5.Vector.fromAngle(this.angle)` crea un vector que apunta en la misma dirección que el vehículo.
          * Luego, este vector se aplica como una fuerza. El resultado es que el vehículo acelera en la dirección a la que está apuntando su "nariz".

3.  **Fricción/Arrastre**: Añadí `this.velocity.mult(0.99)` en el método `update()`. Esto frena muy ligeramente el vehículo en cada fotograma. Sin esto, una vez que aceleras, el vehículo nunca se detendría (como en el espacio profundo). Esta pequeña fricción hace que el control se sienta más "jugable" y menos rígido.

```js
let vehicle;

function setup() {
  createCanvas(640, 360);
  vehicle = new Vehicle(width / 2, height / 2);
}

function draw() {
  background(220);

  // --- NUEVA LÓGICA DE CONTROLES ---

  // 1. Girar el vehículo
  if (keyIsDown(LEFT_ARROW)) {
    vehicle.turn(-0.05); // Pasamos un valor negativo para girar a la izquierda
  }
  if (keyIsDown(RIGHT_ARROW)) {
    vehicle.turn(0.05);  // Pasamos un valor positivo para girar a la derecha
  }

  // 2. Aplicar empuje (aceleración)
  // El código de la barra espaciadora es 32
  if (keyIsDown(32)) {
    vehicle.thrust();
  }

  // Actualizamos y dibujamos el vehículo
  vehicle.update();
  vehicle.checkEdges();
  vehicle.display();

  // Mantenemos el medidor de velocidad
  let speedMeter = vehicle.velocity.mag();
  fill(0);
  textSize(16);
  noStroke();
  text("Velocidad: " + speedMeter.toFixed(2), 10, 20);
}


// --- Clase Vehicle (con cambios importantes) ---

class Vehicle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    
    // ¡NUEVO! Añadimos una propiedad para el ángulo y la velocidad de giro
    this.angle = 0; // El ángulo de orientación, independiente de la velocidad
    
    this.r = 6;
    this.topspeed = 6;
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // ¡NUEVO! Un método para aplicar empuje (thrust)
  thrust() {
    // Creamos un vector de fuerza a partir del ángulo actual del vehículo
    // p5.Vector.fromAngle() crea un vector unitario (longitud 1) a partir de un ángulo.
    let force = p5.Vector.fromAngle(this.angle);
    // Podemos multiplicar para darle más o menos potencia al empuje
    force.mult(0.1);
    this.applyForce(force);
  }
  
  // ¡NUEVO! Un método para girar
  turn(angle) {
    this.angle += angle;
  }

  update() {
    this.velocity.add(this.acceleration);
    // ¡NUEVO! Añadimos un poco de "fricción" o "arrastre" (damping)
    // para que el vehículo no acelere infinitamente y se sienta más controlable.
    // Multiplicar por un número < 1 en cada frame lo frena lentamente.
    this.velocity.mult(0.99); 
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  display() {
    // ¡CAMBIO CLAVE! La rotación ahora depende de `this.angle`,
    // no de la dirección de la velocidad (this.velocity.heading()).
    
    push();
    translate(this.position.x, this.position.y);
    // Sumamos PI/2 porque nuestro triángulo lo dibujamos "apuntando hacia arriba"
    rotate(this.angle + PI / 2);

    fill(127);
    stroke(0);
    strokeWeight(1);
    triangle(0, -this.r * 2, -this.r, this.r, this.r, this.r);

    pop();
  }

  checkEdges() {
    if (this.position.x > width + this.r) {
      this.position.x = -this.r;
    } else if (this.position.x < -this.r) {
      this.position.x = width + this.r;
    }
    if (this.position.y > height + this.r) {
      this.position.y = -this.r;
    } else if (this.position.y < -this.r) {
      this.position.y = height + this.r;
    }
  }
}
```

</details>

---

<img width="500" src="https://github.com/user-attachments/assets/3ba14821-5ea6-4c00-93e2-cab2f3a732ff">

Este modelo es fundamentalmente diferente a los anteriores. Pasamos de simular un solo punto a simular un **cuerpo rígido con partes móviles**.

<details>
  <summary>Experimento 3 vehiculo motocicleta</summary><br>

1.  **Un Sistema de Dos Puntos**: La motocicleta no tiene una única `position`. En su lugar, se define por las posiciones de `this.rearWheel` y `this.frontWheel`. Todos los demás atributos (como la dirección `heading` y la velocidad `speed`) actúan sobre estos dos puntos.

2.  **La Magia del Giro**: La parte más importante está en la función `update()`:

      * Primero, movemos la rueda delantera. Su dirección de movimiento no es la del chasis, sino la del chasis **más** el ángulo del manubrio (`this.heading + this.steerAngle`). Aquí es donde ocurre el "giro".
      * Luego, la rueda trasera tiene que seguirla. Lo hacemos calculando dónde debería estar para mantener la distancia del `chassisLength` con la nueva posición de la rueda delantera. Esto hace que la parte trasera de la moto sea "arrastrada" por la delantera, creando una curva de giro muy natural.
      * Finalmente, actualizamos el `heading` (la dirección del chasis) para que refleje el nuevo ángulo entre las dos ruedas.

3.  **Visualización Compuesta**: El método `display()` también es más complejo. Ya no basta con un solo `translate` y `rotate`. Dibujamos el chasis y las ruedas en sus posiciones absolutas. Para el manubrio, usamos `push()`, `translate()` y `rotate()` para dibujarlo en la posición de la rueda delantera y con la orientación correcta, demostrando visualmente cómo está girando.


```js
let moto;

function setup() {
  createCanvas(640, 480);
  // Creamos la moto en el centro de la pantalla
  moto = new Motorcycle(width / 2, height / 2);
}

function draw() {
  background(220);

  // --- CONTROLES ---
  // Acelerador con la flecha de arriba
  if (keyIsDown(UP_ARROW)) {
    moto.accelerate(0.1);
  }
  
  // Giro con las flechas izquierda y derecha
  if (keyIsDown(LEFT_ARROW)) {
    moto.steer(-0.05);
  }
  if (keyIsDown(RIGHT_ARROW)) {
    moto.steer(0.05);
  }

  moto.update();
  moto.checkEdges();
  moto.display();
  
  // Información en pantalla
  fill(0);
  noStroke();
  textSize(14);
  text("Usa las flechas para moverte", 10, 20);
  text(`Velocidad: ${moto.speed.toFixed(2)}`, 10, 40);
  text(`Ángulo de giro: ${(degrees(moto.steerAngle)).toFixed(1)}°`, 10, 60);
}


// --- Clase Motorcycle ---

class Motorcycle {
  constructor(x, y) {
    this.chassisLength = 30; // Distancia entre las ruedas
    
    // En lugar de una sola posición, ahora tenemos dos: una para cada rueda
    this.rearWheel = createVector(x - this.chassisLength / 2, y);
    this.frontWheel = createVector(x + this.chassisLength / 2, y);
    
    this.speed = 0; // Velocidad de la moto
    this.heading = 0; // Dirección general de la moto (el ángulo del chasis)
    this.steerAngle = 0; // El ángulo de giro del manubrio
    
    this.maxSteerAngle = PI / 4; // Límite de giro (45 grados)
    this.maxSpeed = 5;
  }

  // --- MÉTODOS DE CONTROL ---
  
  accelerate(amount) {
    this.speed += amount;
    if (this.speed > this.maxSpeed) {
      this.speed = this.maxSpeed;
    }
  }
  
  steer(amount) {
    this.steerAngle += amount;
    // Limitar el ángulo de giro para que no sea irreal
    if (this.steerAngle > this.maxSteerAngle) {
      this.steerAngle = this.maxSteerAngle;
    } else if (this.steerAngle < -this.maxSteerAngle) {
      this.steerAngle = -this.maxSteerAngle;
    }
  }
  
  // --- LÓGICA PRINCIPAL ---

  update() {
    // 1. La rueda delantera se mueve
    // Su dirección es la suma del ángulo del chasis + el ángulo del manubrio
    let frontWheelDirection = this.heading + this.steerAngle;
    let velocity = p5.Vector.fromAngle(frontWheelDirection);
    velocity.mult(this.speed);
    this.frontWheel.add(velocity);

    // 2. La rueda trasera "persigue" a la delantera
    // Calculamos el vector que va de la rueda delantera a la trasera
    let dir = p5.Vector.sub(this.rearWheel, this.frontWheel);
    // Ajustamos la longitud de ese vector para que sea igual a la del chasis
    dir.setMag(this.chassisLength);
    // La nueva posición de la rueda trasera es la de la delantera más ese vector
    this.rearWheel = p5.Vector.add(this.frontWheel, dir);

    // 3. Recalculamos el 'heading' general de la moto
    // Es el ángulo del vector que va de la rueda trasera a la delantera
    this.heading = p5.Vector.sub(this.frontWheel, this.rearWheel).heading();

    // 4. Aplicamos fricción para que la moto se detenga eventualmente
    this.speed *= 0.99; 
    // Y el manubrio tiende a enderezarse
    this.steerAngle *= 0.95; 
  }

  display() {
    // Dibujamos el chasis (el cuerpo de la moto)
    stroke(0);
    strokeWeight(4);
    line(this.rearWheel.x, this.rearWheel.y, this.frontWheel.x, this.frontWheel.y);

    // Dibujamos las ruedas
    strokeWeight(2);
    fill(127);
    circle(this.rearWheel.x, this.rearWheel.y, 16);
    circle(this.frontWheel.x, this.frontWheel.y, 16);
    
    // Dibujamos el "manubrio" para visualizar el giro
    push();
    translate(this.frontWheel.x, this.frontWheel.y);
    // Lo rotamos con la dirección completa de la rueda delantera
    rotate(this.heading + this.steerAngle);
    stroke(255, 0, 0); // Rojo para que se note
    strokeWeight(3);
    line(-10, 0, 10, 0);
    pop();
  }

  checkEdges() {
    if (this.frontWheel.x > width) {
        this.frontWheel.x = 0;
        this.rearWheel.x = 0;
    } else if (this.frontWheel.x < 0) {
        this.frontWheel.x = width;
        this.rearWheel.x = width;
    }
    if (this.frontWheel.y > height) {
        this.frontWheel.y = 0;
        this.rearWheel.y = 0;
    } else if (this.frontWheel.y < 0) {
        this.frontWheel.y = height;
        this.rearWheel.y = height;
    }
  }
}
```

</details>

---

</details>

<details>
  <summary>Actividad 4 Relacion con el marco motion 101</summary><br>

Es momento de retomar lo que has aprendido en las unidades previas e integrarlo con los nuevos conceptos de esta unidad. Observa detenidamente la siguiente simulación: [Motion 101 con fuerzas](https://editor.p5js.org/juanferfranco/sketches/jebkEAUpR)

Identifica motion 101. 

- ¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas? Trata de recordar por qué es necesario hacer esta modificación.

  -  En el mover.js está el Motion 101:

     ```js
     update() {
        this.velocity.add(this.acceleration);
        this.position.add(this.velocity);
        /*
        this.angleAcceleration = this.acceleration.x / 10.0;
        this.angleVelocity += this.angleAcceleration;
        this.angleVelocity = constrain(this.angleVelocity, -0.1, 0.1);
        this.angle += this.angleVelocity;
        */
        this.acceleration.mult(0);
      }
     ```
     - La aceleración afecta a la velocidad, y la velocidad afecta a la posición en cada fotograma

     - La modificación crucial para manejar fuerzas acumulativas es reiniciar el vector de aceleración a cero al final de cada ciclo de actualización con `this.acceleration.mult(0);`

     **¿Por qué es necesario?**
     
     El `draw()` loop funciona como una serie de "fotogramas" o instantes en el tiempo. En cada fotograma, queremos calcular la **fuerza neta** que actúa sobre el objeto. Esta fuerza neta es la suma de todas las fuerzas individuales (en este caso, solo hay una, la del atractor, pero podrían ser más).
      
     El proceso es así:
      
     1.  **Acumulación**: En el `draw()` loop, llamamos a `movers[i].applyForce(force)`. Este método **suma** la fuerza al vector `this.acceleration`. Si hubiera más fuerzas, las sumaríamos todas aquí.
     2.  **Aplicación**: En `movers[i].update()`, usamos ese vector de aceleración acumulado para modificar la velocidad.
     3.  **Reinicio**: Al final de `update()`, hacemos `this.acceleration.mult(0)`. Esto es vital. Si no lo hiciéramos, la fuerza del fotograma actual se quedaría "pegada" y se sumaría a la fuerza del siguiente fotograma, y del siguiente, causando una aceleración infinita y poco realista. Reiniciar la aceleración nos asegura que en cada nuevo fotograma empezamos el cálculo de fuerzas desde cero.
        

- Identifica dónde está el Attractor en la simulación. 

  - El `Attractor` es el círculo grande, gris y estacionario que se dibuja en el centro del lienzo. Se crea en `sketch.js` con la línea `attractor = new Attractor();`

- Cambia el color de este.

  - para cambiar el color del Attractor en attractor.js:

    ```js
    // Method to display
    display() {
      ellipseMode(CENTER);
      stroke(0);
      if (this.dragging) {
        fill(50);
      } else if (this.rollover) {
        fill(100);
      } else {
        fill(175, 200); // cambiar a fill(255, 150, 0); para naranjado
      }
      ellipse(this.position.x, this.position.y, this.mass * 2);
    }
    ```

Observa que el Attractor tiene dos atributos `this.dragging` y `this.rollover`. Estos atributos no se modifican en el código, pero permitirían mover el attractor con el mouse y cambiar su color cuando el mouse está sobre él. 

- ¿Cómo podrías modificar el código para que esto funcione? considera las funciones que ofrece p5.js para [interactuar con el mouse](https://p5js.org/reference/).

  - Para que `this.dragging` y `this.rollover` funcionen, necesitamos usar las funciones de eventos de mouse de p5.js. La idea es:

    1.  **Detectar Rollover**: En cada `draw()`, debemos comprobar si el mouse está sobre el atractor.
    2.  **Detectar Click**: Cuando se presiona el mouse, debemos comprobar si está sobre el atractor para iniciar el arrastre (`dragging`).
    3.  **Detectar Arrastre**: Mientras el mouse se arrastra, si el arrastre está activo, movemos el atractor.
    4.  **Detectar Liberación**: Cuando se suelta el botón, detenemos el arrastre.

    [Cambios realizados en el ejemplo](https://editor.p5js.org/DanielZafiro/sketches/diqZAWXIV)

    <img width="500" src="https://github.com/user-attachments/assets/bd87b79a-8e1d-4679-9b97-ba4e3c7cb3f4">


</details>

<details>
  <summary>Actividad 5 Coordenadas Polares</summary><br>

Explora otro sistema de coordenadas útil cuando se trabaja con ángulos. Se trata de las coordenadas polares.

Considera esta simulación de [coordenadas polares](https://editor.p5js.org/juanferfranco/sketches/fE5rCtDS1):

Observa de nuevo esta parte del código 

```js
function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  // Convert polar to cartesian
  let x = r * cos(theta); // <--
  let y = r * sin(theta); // <--
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, x, y); // <--
  circle(x, y, 48); // <--
  theta += 0.02;
}
```

<img width="300" src="https://github.com/user-attachments/assets/ebe5ee79-6a6d-4012-a23e-2b3e07e7c4b7">


- ¿Cuál es la relación entre `r` y `theta` con las posiciones `x` y `y`? Puedes repasar entonces la definición de coordenadas polares y cómo se convierten a coordenadas cartesianas.

  - La relación es la definición misma de la conversión de coordenadas polares a cartesianas, y se basa en trigonometría básica.
    
    * **Coordenadas Polares (`r`, `θ`):** Describen un punto en el espacio por su **distancia** al origen (`r`, el radio) y su **ángulo** de rotación alrededor de ese origen (`θ`, theta). Es como decir "camina 5 metros en un ángulo de 45 grados".
   
    * **Coordenadas Cartesianas (`x`, `y`):** Describen el mismo punto por su posición en los ejes horizontal (`x`) y vertical (`y`). Es como decir "camina 3.5 metros al este y luego 3.5 metros al norte".
   
      La simulación usa las siguientes fórmulas para convertir de polar a cartesiano:
      
      $$x = r \cdot \cos(\theta)$$
      
      $$y = r \cdot \sin(\theta)$$

      Visualmente, `r` es la hipotenusa de un triángulo rectángulo, y `θ` es el ángulo en el origen. Las coordenadas `x` (cateto adyacente) e `y` (cateto opuesto) se calculan usando las funciones trigonométricas `coseno` y `seno`.

      En el código, `r` es un valor fijo (`height * 0.25`), por lo que la distancia al centro no cambia. Lo que sí cambia es `theta` (`theta += 0.02`), que aumenta un poco en cada fotograma. Esto hace que el punto gire alrededor del centro a una distancia constante, creando un movimiento circular perfecto.


ahora, Modifica la función `draw()` asi:

```js
 function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  let v = p5.Vector.fromAngle(theta); // <--
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, x, y);
  circle(v.x, v.y, 48); // <--
  theta += 0.02;
}
```

- ¿Qué ocurre? ¿Por qué?

  - <img width="325" height="279" alt="Arc_cJKkAY6rib" src="https://github.com/user-attachments/assets/86b261f9-2c45-4872-988f-4162ddc53046" />

  
  - error `ReferenceError: x is not defined` ocurre porque en esa primera modificación, el código **elimina** las líneas que creaban las variables `x` e `y`:
    ```js
    // Estas líneas fueron borradas en la primera modificación 
    let x = r * cos(theta);
    let y = r * sin(theta);
    ```
    Y las reemplazamos con:
    ```js
    let v = p5.Vector.fromAngle(theta);
    ```
    Como las variables `x` e `y` ya no existen, cuando el programa llega a la instrucción `line(0, 0, x, y);` se detiene y dice "No sé qué son ni 'x' ni 'y'".

    
Ahora realiza esta modificación:

```js
 function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  let v = p5.Vector.fromAngle(theta,r); // <--
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, v.x, v.y); // <--
  circle(v.x, v.y, 48); // <--
  theta += 0.02;
}
```

<img width="300" src="https://github.com/user-attachments/assets/7ad80d84-d499-4e52-bd32-78dba6dcf389">


- ¿Qué ocurre aquí? ¿Por qué?

  - La simulación se comporta **exactamente igual que la versión original**. El círculo vuelve a trazar su órbita grande alrededor del centro del lienzo.
    
    La nueva línea clave es:

    `let v = p5.Vector.fromAngle(theta, r);`

    Esta es la versión de la función con dos argumentos. Lo que hace es:

    1.  Crea un vector apuntando en la dirección del ángulo `theta`.
    2.  Establece la magnitud (longitud) de ese vector para que sea igual al segundo argumento, `r`.
   
    Esto es, en efecto, un **atajo muy conveniente** que p5.js nos ofrece para hacer la conversión de polar a cartesiano. Esta única línea de código es matemáticamente equivalente a las dos líneas originales:

    ```js
    // Esto:
    let v = p5.Vector.fromAngle(theta, r);
    
    // Es funcionalmente lo mismo que esto:
    let x = r * cos(theta);
    let y = r * sin(theta);
    // (donde v.x sería igual a x, y v.y sería igual a y)
    ```
    
    Como ahora las funciones `line()` y `circle()` usan `v.x` y `v.y`, que contienen los mismos valores que las variables `x` e `y` originales, el resultado visual es idéntico.

    Este ejercicio demuestra una forma más moderna y orientada a objetos (usando vectores) de lograr el mismo objetivo.



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





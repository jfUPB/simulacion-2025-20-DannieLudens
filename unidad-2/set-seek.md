# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1

#### ¿Cómo funciona la suma dos vectores en p5.js?

En p5js los vectores se representan con objetos `p5.Vector`, que contienen componentes `x`, `y` (y opcionalmente `z`). Cuando sumamos dos vectores, estamos sumando sus componentes una a una:

```js
position.add(velocity);
```

esto equivaldria a hacer:

```js
position.x = position.x + velocity.x;
position.y = position.y + velocity.y;
```
el vector `velocity` cambia la posición del vector `position`, actuando como su desplazamiento en cada frame del `draw()` por eso podemos ver el movimeinto del bouncing ball 


#### ¿Por qué esta línea `position = position + velocity;` no funciona?

 porque `position` y `velocity` son objetos de tipo `p5.Vector`, no números simples, n JavaScript, no se pueden sumar objetos directamente con `+` porque son cadenas strings

 por eso la manera correcta de sumar vectores en p5js es con

```js
position.add(velocity);
```

el `.add` es suma de vectores

mas informacion en la [Documentacion de p5.vector](https://p5js.org/es/reference/p5/p5.Vector/)

### Actividad 2

Convertir el Random Walker para que use vectores ya que el codigo sabe sumar escalares pero no sabe sumar vectores:

<details>
  <summary>Random Walker usando Vectores</summary>

```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.position = createVector(width / 2, height / 2); // cambia
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x++;  //cambia
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y++;
    } else {
      this.position.y--;
    }
  }
}

```
</details>

El cambio fue este en la clase walker donde se usaban coordenadas separadas

```js
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }
```

```js
 step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
```

se cambió por esta implementacion con vectores como una sola entidad en vez de dos variables separadas:

el vector es `this.position = createVector...` este vector se asigna a position del propio objeto ese this es como un self

```js
  constructor() {
    this.position = createVector(width / 2, height / 2); 
  }
```

```js
  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x++;  //cambia
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y++;
    } else {
      this.position.y--;
    }
  }
```

por que es util hacer esta implementacion con vectores:

por la escalabilidad ya quepermite construir sistemas de movimiento más complejos (como fuerza, gravedad, fricción) sin tener que manejar `x` y `y` por separado

### Actividad 3 

#### ¿Qué resultado esperas obtener en el programa anterior?

Esperaba que se imprimiera en consola

```
(6,9)
(20,30)
Only once
```

#### ¿Qué resultado obtuviste?

```
p5.Vector Object : [6, 9, 0] 
p5.Vector Object : [20, 30, 0] 
Only once 
```

#### Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.

Metáfora rápida

Por valor: Le das una fotocopia a alguien → pueden rayarla, pero tu original está intacto.

Por referencia: Le das el original → si lo cambian, tú también lo ves cambiado.


Por Valor Aquí `num` no cambia porque los números en JavaScript se pasan por valor:
```js
let num = 5;

function cambiarValor(n) {
  n = 10;
}

cambiarValor(num);
console.log(num);  // ➜ Sigue siendo 5
```

Por referencia `vec` sí cambia, porque los objetos (como los vectores) se pasan por referencia:
```js
let vec = createVector(5, 5);

function mover(v) {
  v.x = 10;
  v.y = 20;
}

mover(vec);
console.log(vec.toString());  // ➜ [10, 20, 0]
```


#### ¿Qué tipo de paso se está realizando en el código?

Se está utilizando por referencia ya que el objeto position (un p5.Vector) se pasa a la función `playingVector(v)` como una referencia. Por eso, cuando se modifican sus propiedades `x` y `y`, los cambios se reflejan fuera de la función.

#### ¿Qué aprendiste?

Experimenté con el `.copy()` para crear una copia del vector sin afectar la direccion original asi:

```js
let position;

function setup() {
    createCanvas(400, 400);
    position = createVector(6, 9); // crea vector y guarda el valor en variable position
    console.log(position.toString()); // (6, 9)

    playingVector(position); // modifica el vector original por referencia
    console.log(position.toString()); // (20, 30)

    playingCopyVector(position); // modifica solo una copia
    console.log(position.toString()); // (20, 30) – el original no cambia

    noLoop();
}

// modifica por referencia el vector que recibe como parámetro
function playingVector(v){
    v.x = 20;
    v.y = 30;
}

// crea una copia del vector y modifica la copia, no el original
function playingCopyVector(v){
    let pos = v.copy(); // crea una copia del vector
    pos.x = 50;
    pos.y = 20;
}

function draw() {
    background(220);
    console.log("Only once");
}
```

lo que me devolvio en consola fue este:

```
p5.Vector Object (6, 9)
p5.Vector Object (20, 30)
p5.Vector Object (20, 30)
Only once
```

Cuando se pasa un `p5.Vector` a una función se pasa por referencia significa que cualquier modificación afectará al objeto original

Para evitar modificar el original, es necesario hacer una copia explícita con `v.copy()`

Esta copia es completamente independiente del objeto original, y permite experimentar o aplicar transformaciones sin efectar al original

---

### Actividad 4

#### Respuesta a preguntas:

<details>
 <summary>¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?</summary> 


### **¿Para qué sirve el método `mag()`?**

El método `mag()` de la clase `p5.Vector` se usa para calcular **la magnitud (o módulo, o longitud)** del vector. Es decir, te dice **qué tan largo es el vector**, sin importar su dirección.

En dos dimensiones, si tienes un vector como:

```js
let v = createVector(3, 4);
```

Entonces:

```js
v.mag(); // retorna 5
```

Esto es porque la magnitud se calcula usando el **Teorema de Pitágoras**:

```
mag = √(x² + y²) = √(3² + 4²) = √(9 + 16) = √25 = 5
```

En tres dimensiones, simplemente se agrega el componente `z²`.

---

#### **¿Qué es `magSq()` y cuál es la diferencia?**

El método `magSq()` hace **lo mismo que `mag()` pero sin hacer la raíz cuadrada**.

Usando el mismo ejemplo:

```js
let v = createVector(3, 4);
v.magSq(); // retorna 25
```

**Entonces:**

* `mag()` te da **la magnitud real**.
* `magSq()` te da **el cuadrado de la magnitud**.

---

#### **¿Cuál es más eficiente?**

**`magSq()` es más eficiente** porque:

* No tiene que calcular una raíz cuadrada (`√`), que es una operación **costosa computacionalmente**.
* A veces no necesitas la magnitud exacta, sino simplemente **comparar magnitudes**. En esos casos puedes usar `magSq()` para ahorrar recursos.

#### Ejemplo de comparación eficiente:

Supongamos que quieres saber si un vector `v` tiene una magnitud mayor que 10:

```js
if (v.mag() > 10) {
  // hace algo
}
```

Esto funciona, pero es más eficiente escribir:

```js
if (v.magSq() > 100) {
  // hace algo
}
```

Evitas así el cálculo de la raíz cuadrada.

---

**Resumen:**

Como diseñadores de obras generativas no necesitamos tener tanta exactitud como si lo requeriria por ejemplo un ingeniero industrial o mecanico por lo que la implementacion mas apropiada para nuestro contexto es usar `magSq()` 

| Método         | ¿Qué hace?                           | ¿Cuándo usarlo?                                          |
| -------------- | ------------------------------------ | -------------------------------------------------------- |
| `mag()`        | Devuelve la longitud real del vector | Cuando necesitas el valor exacto de la magnitud          |
| `magSq()`      | Devuelve el cuadrado de la longitud  | Cuando solo necesitas comparar magnitudes, no la raíz    |
| **Eficiencia** | `magSq()` es más eficiente           | Evita la raíz cuadrada, ideal para comparaciones rápidas |

</details>

---

<img width="600" src="https://github.com/user-attachments/assets/b9f03ceb-b201-4653-90ea-c975abb8d364" />

<details>
 <summary>¿Para qué sirve el método normalize()?</summary>


> Este método convierte el vector en un vector unitario, es decir, mantiene su dirección pero ajusta su longitud a 1. Sirve para trabajar con direcciones puras sin importar la magnitud original.



**Ejemplo: Comparando un vector original con su versión normalizada**

Este sketch dibuja un vector desde el centro de la pantalla hacia el mouse, y también su versión normalizada (es decir, una flecha con la misma dirección pero de longitud 100 píxeles)

[Sketch.js del ejemplo de normalize()](https://editor.p5js.org/DanielZafiro/sketches/nJG9Fyy5M)

<details>
  <summary>sktech.js</summary>

```js
function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(240);
  translate(width / 2, height / 2); // origen en el centro

  // Vector desde el centro hacia el mouse
  let mouse = createVector(mouseX - width / 2, mouseY - height / 2);

  // Copiamos ese vector para normalizarlo sin alterar el original
  let dir = mouse.copy();
  dir.normalize(); // lo convertimos en vector unitario
  dir.mult(100);   // le damos una magnitud de 100 píxeles

  // Dibujamos el vector original en rojo
  stroke(255, 0, 0);
  strokeWeight(3);
  line(0, 0, mouse.x, mouse.y);
  fill(255, 0, 0);
  push();
  fill(255);
  text("Original", mouse.x + 5, mouse.y);
  pop();

  // Dibujamos el vector normalizado en azul
  stroke(0, 0, 255);
  strokeWeight(2);
  line(0, 0, dir.x, dir.y);
  fill(0, 0, 255);
  push();
  fill(255);
  text("Normalizado", dir.x + 5, dir.y);
  pop();
}
```

</details>

---

#### **¿Para qué sirve el método `normalize()`?**

El método `normalize()` de un `p5.Vector` **convierte el vector actual en un vector unitario**, es decir, **mantiene su dirección pero cambia su magnitud a 1**.

---

#### **¿Qué significa esto con un ejemplo práctico?**

Supongamos que tienes un vector:

```js
let v = createVector(3, 4);
```

Sabemos que su magnitud es 5 (como ya vimos con `mag()`).

Si llamas:

```js
v.normalize();
```

Entonces el vector `v` se transforma en uno con **la misma dirección**, pero de **magnitud 1**:

```js
console.log(v); // imprime algo como (0.6, 0.8)
```

¿Por qué? Porque:

```
x = 3 / 5 = 0.6
y = 4 / 5 = 0.8
```

Y √(0.6² + 0.8²) = 1

---

#### **¿Por qué es útil normalizar un vector?**

Normalizar sirve cuando te **interesa la dirección**, pero **no te importa la distancia**. Algunos usos típicos en obras generativas o simulaciones:

* **Controlar la velocidad de un objeto**: puedes usar la dirección normalizada y luego multiplicarla por una velocidad deseada.

  ```js
  let direction = createVector(3, 4);
  direction.normalize();
  direction.mult(2); // ahora es un vector de magnitud 2 en la misma dirección
  ```

* **Evitar que se acumulen efectos no deseados de magnitudes altas**, por ejemplo en sistemas de partículas o steering behaviors.

---

#### **¿Qué pasa si normalizas un vector de magnitud 0?**

Si intentas normalizar un vector con magnitud 0, el método no puede determinar una dirección, así que **el vector queda igual (0, 0)**. Por eso es buena práctica verificar si la magnitud es mayor a 0 antes de normalizar.

---

#### **Resumen:**

| Método        | ¿Qué hace?                                                    | ¿Cuándo usarlo?                                                |
| ------------- | ------------------------------------------------------------- | -------------------------------------------------------------- |
| `normalize()` | Convierte un vector en uno de magnitud 1 (mantiene dirección) | Cuando quieres usar o aplicar una dirección sin cambiar tamaño |
 
</details>

---

<details>
   <summary>Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?</summary>


> Sirve para comparar la dirección de dos vectores, te dice qué tanto apuntan en la misma dirección
> 
> Si el resultado es positivo, van más o menos en la misma dirección. Si es cero, son perpendiculares. Si es negativo, van en direcciones opuestas.

<details>
  <summary>más elaborado y ejemplo</summary>

El método `dot()` en p5.js **calcula el producto punto (o producto escalar)** entre dos vectores.

---

#### ¿Qué es el producto punto?

Es una operación matemática que **toma dos vectores y devuelve un solo número (un escalar)**. En el caso de p5.js:

```js
let a = createVector(1, 2);
let b = createVector(3, 4);

let resultado = a.dot(b);  // resultado = 1*3 + 2*4 = 11
```

---

#### ¿Qué información nos da el producto punto?

* Si el resultado es **positivo**, los vectores apuntan en una dirección **similar**.
* Si es **cero**, los vectores son **perpendiculares** (forman un ángulo de 90°).
* Si es **negativo**, los vectores apuntan en **direcciones opuestas**.

</details>
 
</details>

---

<details>
  <summary>El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?</summary>

---

#### Que hace cada versión:

* **Versión de instancia (`a.dot(b)`):**
  Se llama **desde un vector existente**, y recibe otro vector como argumento.

  ```js
  let a = createVector(1, 2);
  let b = createVector(3, 4);
  let resultado = a.dot(b);  // 1*3 + 2*4 = 11
  ```

* **Versión estática (`p5.Vector.dot(a, b)`):**
  Se llama **desde la clase p5.Vector**, y recibe **dos vectores como argumentos**.

  ```js
  let a = createVector(1, 2);
  let b = createVector(3, 4);
  let resultado = p5.Vector.dot(a, b);  // también da 11
  ```

---

#### ¿Cuál usar?

* Ambas hacen exactamente lo mismo y dan el mismo resultado.
* La diferencia es **cómo se llama al método**:

  * ¿Ya tienes un vector y quieres compararlo con otro? Usa la de instancia.
  * ¿Tienes dos vectores y quieres operarlos directamente? Usa la estática.

 
</details>

---

<img src="https://github.com/user-attachments/assets/4e106128-56fd-49e1-bed0-34ae0809fe82" width="600">

<details>
  <summary>Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.</summary>

> Esta pregunta es clave para comprender el comportamiento espacial de los vectores.

 éste es un ejemplo simple en p5.js usando `WEBGL` para visualizar el producto cruzado (`cross`) de dos vectores en el espacio 3D:

* Un vector rojo (`v1`) hacia la derecha.
* Un vector verde (`v2`) hacia arriba.
* El vector azul, que es el producto cruzado, apunta hacia adelante (en el eje Z), saliendo del plano XY.

Puedes girar la vista con el mouse (gracias a `orbitControl()`) para comprobar que el azul realmente es perpendicular.

[Link al sketch de ejemplo para el producto cruz](https://editor.p5js.org/DanielZafiro/sketches/URuIJxblx)

<details>
   <summary>sketch.js del ejemplo de prodcuto cruzado</summary>

```js
let v1, v2, crossProduct;

function setup() {
  createCanvas(600, 600, WEBGL);

  // Dos vectores en el plano XY
  v1 = createVector(100, 0, 0);   // Vector hacia la derecha
  v2 = createVector(0, 100, 0);   // Vector hacia arriba

  // Producto cruzado (perpendicular a ambos, en Z)
  crossProduct = v1.cross(v2);
}

function draw() {
  background(240);
  orbitControl();  // Permite rotar la cámara con el mouse

  strokeWeight(4);
  
  // Ejes de referencia
  drawAxis();

  // Dibuja v1 (rojo)
  stroke(255, 0, 0);
  line(0, 0, 0, v1.x, -v1.y, v1.z);

  // Dibuja v2 (verde)
  stroke(0, 255, 0);
  line(0, 0, 0, v2.x, -v2.y, v2.z);

  // Dibuja producto cruzado (azul)
  stroke(0, 0, 255);
  line(0, 0, 0, crossProduct.x, -crossProduct.y, crossProduct.z);
}

function drawAxis() {
  strokeWeight(1);

  // Eje X - rojo
  stroke(255, 0, 0);
  line(0, 0, 0, 150, 0, 0);

  // Eje Y - verde
  stroke(0, 255, 0);
  line(0, 0, 0, 0, -150, 0);

  // Eje Z - azul
  stroke(0, 0, 255);
  line(0, 0, 0, 0, 0, 150);
}

```
</details>

---

El **producto cruz** (o **cross product**) entre dos vectores produce **un nuevo vector perpendicular a ambos**. Es decir, genera un vector que apunta "hacia afuera" del plano que forman los dos vectores originales.

#### Intuición geométrica:

* **Orientación:**
  El vector resultante es **ortogonal (perpendicular)** al plano que forman los dos vectores.
  En 3D, esto significa que si los dos vectores están sobre una hoja de papel, el producto cruzado apunta **hacia arriba o hacia abajo** del papel, dependiendo del orden de los vectores (la **regla de la mano derecha** te indica el sentido).

* **Magnitud:**
  La longitud del vector resultante es igual al **área del paralelogramo** que forman los dos vectores.
  Si los vectores son paralelos, el área es cero, y por tanto el producto cruz es un vector nulo (0, 0, 0).

---

### Ejemplo visual (conceptual):

Imagina dos vectores en 3D:

* `a = (1, 0, 0)` apunta hacia la derecha (eje X)
* `b = (0, 1, 0)` apunta hacia arriba (eje Y)

El producto cruz `a.cross(b)` da como resultado:

* `c = (0, 0, 1)` → un vector que apunta **hacia ti** (eje Z positivo), **perpendicular** a ambos.

</details>

---

<img src="https://github.com/user-attachments/assets/05a18f1a-94e5-4551-9c23-4c659a2ab090" width="400">

<details>
  <summary>¿Para que te puede servir el método dist()?</summary>

#### ¿Para qué sirve el método dist()?

[Link a p5js ejemplo de dist()](https://editor.p5js.org/DanielZafiro/sketches/kMsg8vIW_)

<details>
  <summary>sketch.js ejemplo de dist()</summary>

```js
let x1 = 100, y1 = 200;
let x2, y2;

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(240);

  x2 = mouseX;
  y2 = mouseY;

  // Dibuja los puntos
  fill(255, 0, 0);
  ellipse(x1, y1, 20, 20);
  fill(0, 0, 255);
  ellipse(x2, y2, 20, 20);

  // Línea entre los puntos
  stroke(0);
  line(x1, y1, x2, y2);

  // Distancia
  let d = dist(x1, y1, x2, y2);
  noStroke();
  fill(0);
  textSize(16);
  text("Distancia: " + nf(d, 1, 2), 10, height - 20);
}

```
</details>

El método `dist()` en p5.js se usa para **calcular la distancia entre dos puntos en el espacio** (ya sea en 2D o 3D). Es especialmente útil cuando quieres saber **qué tan lejos están dos objetos o entidades entre sí**, lo cual es muy común en simulaciones interactivas, animaciones o videojuegos.

#### Aplicaciones típicas de `dist()`:

* Ver si un objeto está lo suficientemente cerca de otro para activar una interacción.
* Detectar colisiones suaves entre partículas o entidades.
* Determinar qué tan lejos se ha desplazado un objeto desde un punto inicial.
* Crear efectos que dependen de la proximidad (como cambiar color, tamaño o velocidad).


</details>

> [!NOTE]
> 
> `dist()` calcula la distancia entre dos vectores, como si fueran puntos en el espacio.
Es útil para saber qué tan lejos están dos objetos, como por ejemplo peces y comida en la [obra generativa](https://editor.p5js.org/DanielZafiro/sketches/6Q7fMKdSM) de la unidad anterior


---

<details>
  <summary>¿Para qué sirven los métodos normalize() y limit()?</summary>

#### ¿Para qué sirven los métodos normalize() y limit()?

Imagina que tienes una linterna (dirección) con un haz de luz (vector):

* `normalize()` → Mantienes el haz del mismo largo, **solo te interesa hacia dónde estás apuntando**.
* `limit()` → Le pones un tope a la intensidad del haz para **que no alumbre más de la cuenta**.

En p5.js, los métodos `normalize()` y `limit()` nos ayudan a trabajar mejor con vectores. El método `normalize()` se usa cuando queremos conservar la dirección de un vector pero no su tamaño, es decir, cuando solo nos interesa **hacia dónde va** un objeto, no **qué tan lejos está**. Esto es útil, por ejemplo, para mover algo con velocidad constante en una dirección específica. Por otro lado, `limit()` nos permite ponerle un tope al tamaño del vector, lo que sirve para controlar **la velocidad máxima** de algo. Es como decirle a un objeto “puedes moverte en esa dirección, pero no más rápido que esto”. Ambos métodos ayudan a tener más control sobre los movimientos y fuerzas en una simulación.

<details>
  <summary>más elaborado</summary>



#### 🎯 ¿Qué son `normalize()` y `limit()` en `p5.Vector`?

Son dos **herramientas especiales** que nos ayudan a **controlar la magnitud de un vector** sin cambiar hacia dónde apunta.

Para entenderlo mejor, imagina que un vector es como **una flecha**:

* La **dirección** de la flecha te dice hacia dónde va.
* El **tamaño (magnitud)** te dice qué tan fuerte o rápido va.

---

#### `normalize()` – “Solo quiero saber a dónde ir”

- **¿Qué hace?**

`normalize()` toma una flecha (vector) y la **hace del mismo tamaño siempre**, sin cambiar su dirección. Ese tamaño será **exactamente 1**.

> Es como si solo te interesara la dirección, pero no qué tan lejos está el objetivo.

- **¿Para qué sirve?**

Para saber **la dirección exacta** a la que quieres moverte o aplicar una fuerza, **sin importar la distancia**.

- **Ejemplo:**

Imagina que estás en un videojuego y quieres que tu personaje **siempre corra hacia el mouse**, pero a una **velocidad constante**, sin importar si el mouse está cerca o lejos.
Entonces haces esto:

```js
let direccion = createVector(mouseX - x, mouseY - y);
direccion.normalize(); // ahora solo tiene dirección, no velocidad
direccion.mult(velocidadConstante); // ahora sí lo hacemos avanzar
```

---

#### `limit()` – “¡No vayas tan rápido!”

- **¿Qué hace?**

`limit()` pone un **límite máximo** al tamaño (magnitud) de un vector.

> Si la flecha es más larga de lo permitido, la acorta. Si ya es más corta, la deja igual.

- **¿Para qué sirve?**

Para **evitar que algo se mueva demasiado rápido** o aplique demasiada fuerza.

- **Ejemplo:**

Si tienes un pez que nada, pero a veces se asusta y acelera demasiado, puedes hacer esto:

```js
pez.velocidad.limit(4); // que nunca vaya más rápido que 4
```

---

#### Comparación sencilla

| Método        | ¿Qué hace?                               | ¿Para qué sirve?                         |
| ------------- | ---------------------------------------- | ---------------------------------------- |
| `normalize()` | Cambia el tamaño a 1                     | Para tener solo dirección, sin velocidad |
| `limit()`     | Pone un tope máximo al tamaño del vector | Para controlar la velocidad o fuerza     |


</details>

</details>

## Actividad 5

### Interpolamos?

<details>
  <summary>Enunciado</summary>

> Vas a tomar como inspiración este ejemplo de la referencia de p5.js:
 
<details>
   <summary>sketch.js </summary>

```js
function setup() {
    createCanvas(100, 100);
}

function draw() {
    background(200);

    let v0 = createVector(50, 50);
    let v1 = createVector(30, 0);
    let v2 = createVector(0, 30);
    let v3 = p5.Vector.lerp(v1, v2, 0.5);
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, 'purple');
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```
    
</details>

Vas a modificarlo para generar este resultado

<img width="436" height="437" alt="image" src="https://juanferfranco.github.io/simulacion-2025-20/_astro/vectorLerp.CTzOmtf1_2iKxEf.webp" />

- Analiza cómo funciona el método `lerp()`.
- Nota que además de la interpolación lineal de vectores, también puedes hacer interpolación lineal de colores con el método `lerpColor()`.

Dedica un tiempo a estudiar cómo se dibuja una flecha en el método `drawArrow()`

> En tu bitácora escribe:
> 
> - El código que genera el resultado que te pedí.
> - ¿Cómo funciona lerp() y lerpColor().
> - ¿Cómo se dibuja una flecha usando drawArrow()?
 
</details>

<details>
  <summary>Respuesta</summary>

<details>
   <summary>sketch.js modificado</summary>

```js
let amt = 0; // valor de interpolación
let increasing = true;

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);

  let v0 = createVector(80, 100); // origen común
  let v1 = createVector(0, 200);  // vector azul (vertical)
  let v2 = createVector(200, 0);  // vector rojo (horizontal)

  // Interpolamos entre v1 y v2
  let v3 = p5.Vector.lerp(v1, v2, amt);

  // Color interpolado entre azul y rojo
  let c = lerpColor(color(0, 0, 255), color(255, 0, 0), amt);

  // Dibujamos las flechas desde el mismo origen
  drawArrow(v0, v1, color(0, 0, 255));    // Azul
  drawArrow(v0, v2, color(255, 0, 0));    // Rojo
  
  drawArrow(v0, v3, c);                   // Flecha interpolada con color cambiante
  
  // Flecha verde dibujada 
  let origenRojo = p5.Vector.add(v0, v2);
  let destinoAzul = p5.Vector.add(v0, v1);
  let flechaVerde = p5.Vector.sub(destinoAzul, origenRojo);
  drawArrow(origenRojo, flechaVerde, color(0, 160, 0));


  // Actualizar `amt` para animar de ida y vuelta
  if (increasing) {
    amt += 0.01;
    if (amt >= 1) increasing = false;
  } else {
    amt -= 0.01;
    if (amt <= 0) increasing = true;
  }
}

// Función para dibujar flechas desde un punto base con un vector
function drawArrow(base, vec, myColor) {
  push();
  stroke(myColor);
  strokeWeight(3);
  fill(myColor);
  translate(base.x, base.y);
  line(0, 0, vec.x, vec.y);
  rotate(vec.heading());
  let arrowSize = 7;
  translate(vec.mag() - arrowSize, 0);
  triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
  pop();
}

```
    
</details>

<img src="https://github.com/user-attachments/assets/6d231370-a46b-4392-9bae-667f9376a802" width="400">

#### Bitácora Actividad 04 - Análisis de vectores y funciones en p5.js

##### Modificaciones realizadas

Se creó una visualización en la que se muestran tres vectores originados desde un mismo punto (`v0`):

- **Vector azul (`v1`)**: vertical.
- **Vector rojo (`v2`)**: horizontal.
- **Vector interpolado (`v3`)**: generado a partir de la interpolación entre `v1` y `v2` usando `p5.Vector.lerp()`.

Adicionalmente, se dibuja una **flecha verde** que representa el vector de diferencia desde la punta del vector rojo hasta la punta del vector azul. Esta flecha no parte del origen común, sino que parte de la punta del vector rojo (`v0 + v2`) y apunta hacia la punta del vector azul (`v0 + v1`).

Se animó el valor `amt` de 0 a 1 y de regreso para mostrar cómo cambia gradualmente el vector interpolado y su color asociado.

---

#### ¿Cómo funciona `lerp()` y `lerpColor()`?

#### `lerp()` en p5.js

* **Significa "linear interpolation" (interpolación lineal).**

* Su sintaxis general es:

  ```javascript
  let valorInterpolado = lerp(valorA, valorB, t);
  ```

* Donde:

  * `valorA` es el punto de inicio,
  * `valorB` es el punto final,
  * `t` es un número entre `0` y `1` que determina **cuánto** nos movemos desde A hacia B.

* Cuando `t = 0` → el resultado es `valorA`.

* Cuando `t = 1` → el resultado es `valorB`.

* Si `t = 0.5` → estamos justo a la mitad.

#### `p5.Vector.lerp(v1, v2, t)`

* Hace lo mismo, pero para **vectores**: genera un vector que se encuentra en algún punto entre `v1` y `v2`.


- **`p5.Vector.lerp(v1, v2, amt)`**  
  Realiza una interpolación lineal entre dos vectores `v1` y `v2`. El valor `amt` indica el porcentaje de interpolación:
  - Si `amt = 0`, devuelve `v1`.
  - Si `amt = 1`, devuelve `v2`.
  - Si `amt = 0.5`, devuelve un punto a mitad de camino entre `v1` y `v2`.

  Este método es útil para crear movimientos suaves o transiciones entre direcciones o posiciones.


#### `lerpColor(colorA, colorB, t)`

* Interpola entre dos colores, devolviendo un color intermedio.
* Ejemplo:

  ```javascript
  lerpColor(color(0, 0, 255), color(255, 0, 0), 0.5);
  // Da como resultado un color violeta (mezcla entre azul y rojo).
  ```

- **`lerpColor(c1, c2, amt)`**  
  Interpola linealmente entre dos colores `c1` y `c2` usando el mismo principio que con los vectores. A medida que `amt` cambia, el color resultante va transicionando entre ambos.

  En el código, esto se utiliza para cambiar dinámicamente el color del vector interpolado entre azul y rojo.

---

##### ¿Cómo se dibuja una flecha usando `drawArrow()`?

La función `drawArrow(base, vec, myColor)` dibuja una flecha desde un punto base (`base`) en la dirección y magnitud de un vector (`vec`) con un color específico (`myColor`).

1. Se traslada el sistema de coordenadas al punto base.
2. Se dibuja una línea desde `(0, 0)` hasta `(vec.x, vec.y)`.
3. Rota el sistema de coordenadas hacia la dirección del vector con `rotate(vec.heading())`
4. Se dibuja un triángulo al final de la línea para simular la punta de la flecha.

Esta implementación permite reutilizar la función para dibujar flechas de cualquier longitud, dirección y color, en cualquier posición del lienzo.


</details>

### Actividad 6

<details>
 <summary>Motion 101</summary>

> **Comportamiento del ejemplo 1.7:**
>
> * Cada vez que se ejecuta `draw()`, el objeto se mueve automáticamente.
> * Su dirección y velocidad inicial son aleatorias.
> * Cuando el objeto sale de la pantalla, reaparece por el lado opuesto (comportamiento tipo *wrap-around*).
> * Esto simula un movimiento continuo, suave y básico.


#### ¿Cuál es el concepto del marco *Motion 101* y cómo se interpreta geométricamente?

**Motion 101** es una forma simple pero poderosa de entender el movimiento en programación mediante el uso de vectores. La idea central es que un objeto en movimiento puede describirse por:

* **Posición (`position`)**
* **Velocidad (`velocity`)**
* *(más adelante también se incluirá la aceleración)*

En este marco, cada fotograma (frame) del programa actualiza la posición del objeto **sumándole la velocidad**. Es decir:

```js
position.add(velocity);
```

Esto se traduce en que:

* La **posición** indica dónde está el objeto.
* La **velocidad** indica hacia dónde se mueve y qué tan rápido lo hace.

> 📐 **Geométricamente**, lo puedes imaginar así:
> El vector de posición es una flecha desde el origen del sistema (0,0) hasta el objeto.
> El vector de velocidad es otra flecha que indica la dirección y magnitud del cambio en posición.
> En cada ciclo del programa, se dibuja una nueva posición siguiendo la dirección de la velocidad.

---

#### ¿Cómo se aplica *Motion 101* en el ejemplo?

En el **ejemplo 1.7** del libro *The Nature of Code*, se aplica el marco **Motion 101** usando dos vectores principales: `position` y `velocity`.

---

####  ¿Qué significa aplicar Motion 101?

Aplicar *Motion 101* significa que el movimiento del objeto se genera simplemente **sumando la velocidad a la posición en cada fotograma**. Esto es lo que hace que el objeto parezca moverse en la pantalla.

En código, se ve así:

```js
this.position.add(this.velocity);
```

---

####  ¿Cómo funciona en el ejemplo?

1. **Al iniciar**, el objeto (`mover`) recibe una posición y una velocidad aleatoria.

   ```js
   this.position = createVector(random(width), random(height));
   this.velocity = createVector(random(-2, 2), random(-2, 2));
   ```

2. **En cada fotograma**, se ejecuta el método `update()`:

   ```js
   this.position.add(this.velocity);
   ```

   Esto hace que el objeto se mueva automáticamente por la pantalla, siguiendo su velocidad.

3. **Luego se dibuja** con `show()`, y se controla que no se salga de los bordes con `checkEdges()`.


#### mover.js

```js
class Mover {
  constructor() {
    // Posición y velocidad aleatorias al iniciar
    this.position = createVector(random(width), random(height));
    this.velocity = createVector(random(-2, 2), random(-2, 2));
  }

  update() {
    // Movimiento: nueva posición = posición + velocidad
    this.position.add(this.velocity);
  }

  show() {
    // Dibujar el objeto como un círculo
    stroke(0);
    strokeWeight(2);
    fill(127);
    circle(this.position.x, this.position.y, 48);
  }

  checkEdges() {
    // Rebote por los bordes: aparece en el lado opuesto
    if (this.position.x > width) {
      this.position.x = 0;
    } else if (this.position.x < 0) {
      this.position.x = width;
    }

    if (this.position.y > height) {
      this.position.y = 0;
    } else if (this.position.y < 0) {
      this.position.y = height;
    }
  }
}
```

#### sketch.js

```js
let mover;

function setup() {
  createCanvas(640, 240);
  mover = new Mover(); // Crear instancia del objeto
}

function draw() {
  background(255);

  mover.update();      // Actualiza posición
  mover.checkEdges();  // Controla los bordes
  mover.show();        // Dibuja el círculo
}
```

[Link al ejemplo 1.7. motion 101](https://editor.p5js.org/natureofcode/sketches/6foX0NUfS)

 
</details>


###  Actividad 7

#### Experimento con la aceleracion

Durante este experimento exploré el efecto de distintos tipos de aceleración sobre un objeto en movimiento:

- **Aceleración constante** genera una trayectoria uniforme con velocidad creciente en una dirección fija.
- **Aceleración aleatoria** produce un movimiento errático, similar al de partículas en un fluido o el viento cambiante.
- **Aceleración hacia el mouse** crea una dinámica de persecución, como si el objeto estuviera “vivo” y consciente del cursor.

Al combinar los tres tipos y alternar entre ellos, pude observar con claridad cómo pequeñas modificaciones en la lógica de la aceleración pueden tener efectos drásticos en la calidad del movimiento. Esta actividad refuerza el principio central de “Motion 101”: definir la aceleración y dejar que el sistema propague sus efectos a través de la velocidad y la posición.


#### Frase del libro:

> “The goal for programming motion is to come up with an algorithm for calculating acceleration and then let the trickle-down effect work its magic.”

#### ¿Qué significa?

Esta frase plantea un principio fundamental del movimiento con vectores:
en lugar de manipular directamente la posición o la velocidad de un objeto, **se diseña una regla (algoritmo) para calcular su aceleración**. A partir de esa aceleración, **el resto del movimiento sucede de forma natural** por el efecto acumulativo:

* La **aceleración** afecta a la **velocidad**
* La **velocidad** afecta a la **posición**

Este efecto en cadena es lo que el autor llama “trickle-down effect”.

---

Ahora vamos a construir el experimento en tres partes. En cada una probaremos un tipo diferente de aceleración y luego haremos observaciones específicas. Comenzamos con el primer tipo.

<details>
  <summary>Parte 1: Aceleración constante</summary>
 


---

#### Parte 1: Aceleración constante

<img src="https://github.com/user-attachments/assets/27bdba57-11e5-4db8-bed3-1e58123aa8d3" width="400">

[link al ejemplo en p5js](https://editor.p5js.org/DanielZafiro/sketches/NqfJtHyLS)

#### Código base

```js
let mover;

function setup() {
  createCanvas(640, 360);
  mover = new Mover();
}

function draw() {
  background(255);
  mover.update();
  mover.edges();
  mover.show();
}

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  update() {
    // Aceleración constante hacia la derecha
    this.acceleration.set(0.05, 0);

    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
  }

  edges() {
    if (this.position.x > width || this.position.x < 0) {
      this.velocity.x *= -1;
    }
    if (this.position.y > height || this.position.y < 0) {
      this.velocity.y *= -1;
    }
  }

  show() {
    fill(127);
    stroke(0);
    ellipse(this.position.x, this.position.y, 32);
  }
}
```

#### Observaciones

* La aceleración constante provoca un **aumento progresivo de la velocidad**.
* El objeto **se mueve cada vez más rápido** en la dirección de la aceleración.
* Como no hay fricción ni límites de velocidad, la velocidad **se acumula indefinidamente**.
* La trayectoria es **lineal**, porque no hay cambios de dirección.
* La aceleración actúa como un empuje constante, como si una fuerza invisible estuviera tirando del objeto sin parar.

---

Perfecto, continuemos con la **Parte 2: Aceleración aleatoria**, manteniendo el mismo enfoque didáctico y progresivo:

---

</details>

<details>
 <summary>Aceleracion aleatoria</summary>
 


#### Parte 2: Aceleración aleatoria

<img src="https://github.com/user-attachments/assets/8eaab402-ca3d-43b6-aaff-bcb2383ac22a" width="400">




[link al ejemplo en p5js](https://editor.p5js.org/DanielZafiro/sketches/LuNcN7z0P)

#### Modificación en el código

Para generar aceleraciones aleatorias, cambiaremos la línea que fija la aceleración constante por una línea que le dé al objeto un empujón en una dirección aleatoria en cada frame:

```js
update() {
  // Aceleración aleatoria en cada frame
  let randomAccel = p5.Vector.random2D();
  randomAccel.setMag(0.2); // Magnitud constante para que no sea caótica
  this.acceleration = randomAccel;

  this.velocity.add(this.acceleration);
  this.position.add(this.velocity);
}
```

#### Observaciones

* La aceleración **cambia de dirección en cada frame**, lo que provoca un movimiento **errático** o **caótico**.
* A pesar del desorden, el objeto no se mueve sin rumbo totalmente al azar: la **velocidad sigue acumulando los efectos anteriores**, lo que suaviza un poco la trayectoria.
* En lugar de quedarse en un lugar o hacer zigzags instantáneos, el objeto **tiende a vagar** por la pantalla como si estuviera **navegando con viento cambiante**.
* Si no hay fricción o límites, la velocidad seguirá creciendo, por lo que con el tiempo el objeto puede moverse más rápido y más lejos.
  (Se puede controlar esto con `velocity.limit(maxSpeed)` si se desea).

Este tipo de movimiento es útil para simular comportamientos como:

* Partículas flotando en un fluido
* Insectos o criaturas pequeñas moviéndose sin rumbo fijo
* Efectos naturales donde hay fuerzas impredecibles (viento, agua, etc.)

---

</details>

<details>
  <summary>Aceleracion hacia el mouse</summary>


#### Parte 3: Aceleración hacia el mouse

<img src="https://github.com/user-attachments/assets/642445a9-b782-41d1-91e4-04773ec597c6" width="400">





[link al ejemplo en p5js](https://editor.p5js.org/DanielZafiro/sketches/7-p3pabDM)

#### Modificación en el código

Para aplicar una aceleración que **apunte hacia el cursor**, crearemos un vector que vaya desde la posición del objeto hacia el mouse y lo usaremos como aceleración.

```js
update() {
  // Aceleración dirigida hacia el mouse
  let mouse = createVector(mouseX, mouseY);
  let dir = p5.Vector.sub(mouse, this.position); // Vector desde el objeto hasta el mouse
  dir.setMag(0.2); // Establecer la magnitud de la aceleración

  this.acceleration = dir;

  this.velocity.add(this.acceleration);
  this.position.add(this.velocity);
}
```

#### Observaciones

* El objeto **acelera cada vez más rápidamente** hacia el cursor, como si estuviera **atraído gravitacionalmente**.
* Si el cursor se mueve, la aceleración cambia de dirección para seguirlo, creando un comportamiento de **persecución dinámica**.
* A medida que el objeto se acerca al cursor, su velocidad puede hacerse muy alta si no se limita, causando que lo sobrepase y tenga que girar para regresar, lo cual genera una especie de **oscilación alrededor del mouse**.
* Si se añade una línea como `this.velocity.limit(5)`, el movimiento se vuelve más fluido y controlado.

Este patrón es muy útil para simular:

* Comportamiento de agentes inteligentes (enemigos, seguidores, partículas inteligentes)
* Imitaciones de gravedad local
* Movimientos dirigidos en sistemas interactivos (como menús que siguen el puntero o visualizaciones de datos interactivas)


---

</details>

<details>
  <summary>Combinacion de aceleraciones</summary>


#### Combinación de los tres tipos de aceleración

#### ¿Cómo se implementa?

Una forma práctica de combinar los tres tipos es alternar entre ellos con una tecla o aplicar cada uno en una situación distinta. Aquí te muestro un ejemplo sencillo donde se puede cambiar entre **aceleración constante**, **aleatoria** y **hacia el mouse** con las teclas `1`, `2` y `3`.

```js
let modo = 1; // Modo inicial

function keyPressed() {
  if (key === '1') modo = 1; // Constante
  if (key === '2') modo = 2; // Aleatoria
  if (key === '3') modo = 3; // Hacia el mouse
}
```

Luego, en el método `update()` del objeto:

```js
update() {
  if (modo === 1) {
    // Aceleración constante
    this.acceleration = createVector(0.05, 0);
  } else if (modo === 2) {
    // Aceleración aleatoria
    this.acceleration = p5.Vector.random2D().mult(0.5);
  } else if (modo === 3) {
    // Aceleración hacia el mouse
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, this.position);
    dir.setMag(0.2);
    this.acceleration = dir;
  }

  this.velocity.add(this.acceleration);
  this.position.add(this.velocity);
  this.velocity.limit(6); // Límite para evitar desbordes
}
```

#### ¿Qué se observa?

* Puedes comparar en tiempo real cómo se comporta el objeto con cada tipo de aceleración.
* Esto te permite **entender cómo cambia la trayectoria** dependiendo de la fuente de aceleración.
* Es ideal para aprender a **modular el movimiento** de agentes en un entorno interactivo.

---

</details>



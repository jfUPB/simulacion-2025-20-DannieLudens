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

### Actividad 4

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

<details>
 <summary>¿Para qué sirve el método normalize()?</summary>


> Este método convierte el vector en un vector unitario, es decir, mantiene su dirección pero ajusta su longitud a 1. Sirve para trabajar con direcciones puras sin importar la magnitud original.

<img width="600" src="https://github.com/user-attachments/assets/b9f03ceb-b201-4653-90ea-c975abb8d364" />

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


<details>
  <summary>Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.</summary>

> Esta pregunta es clave para comprender el comportamiento espacial de los vectores.

 éste es un ejemplo simple en p5.js usando `WEBGL` para visualizar el producto cruzado (`cross`) de dos vectores en el espacio 3D:

<img src="https://github.com/user-attachments/assets/4e106128-56fd-49e1-bed0-34ae0809fe82" width="600">

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


<details>
  <summary>¿Para que te puede servir el método dist()?</summary>


 
</details>

#### ¿Cuál es la interpretación geométrica del producto cruzado (cross())?

El producto cruzado entre dos vectores genera un tercer vector perpendicular al plano definido por los dos originales.
La magnitud del nuevo vector es igual al área del paralelogramo formado por los dos vectores.
La orientación del vector resultante depende del orden de los vectores y sigue la regla de la mano derecha.
Este método es útil para calcular normales en superficies o rotaciones en 3D.




continuare aqui



#### ¿Para qué sirve el método dist()?

`dist()` calcula la distancia entre dos vectores, como si fueran puntos en el espacio.
Es útil para saber qué tan lejos están dos objetos, como por ejemplo peces y comida en una simulación.






#### ¿Para qué sirven los métodos normalize() y limit()?

`normalize()`: ajusta el vector para que tenga magnitud 1 sin cambiar su dirección.

`limit(valor)`: restringe la magnitud máxima del vector al valor dado.
Ambos son útiles para controlar comportamientos como velocidad constante o límite de aceleración en un sistema dinámico.


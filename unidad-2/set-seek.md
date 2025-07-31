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

#### ¿Para qué sirve el método mag()?

El método `mag()` calcula la magnitud (longitud) del vector, es decir, qué tan largo es. Se obtiene aplicando el teorema de Pitágoras:

mag = sqrt(x^2 + y^2 + z^2)

#### ¿Cuál es la diferencia entre mag() y magSq()? ¿Cuál es más eficiente?

`magSq()` calcula la magnitud al cuadrado del vector:

magSq = x^2 + y^2 + z^2

Es más eficiente que `mag()` porque evita la operación de raíz cuadrada, que es más costosa computacionalmente.

Se usa cuando solo se necesita comparar magnitudes, no conocer el valor exacto.

#### ¿Para qué sirve el método `normalize()`?

Este método convierte el vector en un vector unitario, es decir, mantiene su dirección pero ajusta su longitud a 1. Sirve para trabajar con direcciones puras sin importar la magnitud original.

#### ¿Para qué sirve el método `dot()`? 

El método `dot()` indica que tanto dos vectores apuntan en la misma dirección. Si el resultado es alto, van en la misma dirección. Si es cero, son perpendiculares.

En una frase: "Sirve para saber cuán alineados están dos vectores."

Diferencia entre `dot()` estático e instancia

v1.dot(v2): se usa desde una instancia de vector.

p5.Vector.dot(v1, v2): se usa como método estático desde la clase.
Ambas formas hacen lo mismo, pero la forma de uso cambia según el contexto.

#### ¿Cuál es la interpretación geométrica del producto cruzado (cross())?

El producto cruzado entre dos vectores genera un tercer vector perpendicular al plano definido por los dos originales.
La magnitud del nuevo vector es igual al área del paralelogramo formado por los dos vectores.
La orientación del vector resultante depende del orden de los vectores y sigue la regla de la mano derecha.
Este método es útil para calcular normales en superficies o rotaciones en 3D.

#### ¿Para qué sirve el método dist()?

`dist()` calcula la distancia entre dos vectores, como si fueran puntos en el espacio.
Es útil para saber qué tan lejos están dos objetos, como por ejemplo peces y comida en una simulación.

#### ¿Para qué sirven los métodos normalize() y limit()?

`normalize()`: ajusta el vector para que tenga magnitud 1 sin cambiar su dirección.

`limit(valor)`: restringe la magnitud máxima del vector al valor dado.
Ambos son útiles para controlar comportamientos como velocidad constante o límite de aceleración en un sistema dinámico.

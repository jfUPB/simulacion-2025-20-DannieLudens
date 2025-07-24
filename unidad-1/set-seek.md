# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 4

> [!NOTE]
> Bitácora
> 
> - En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios.
> - Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.

Este [Ejemplo](https://juanferfranco.github.io/simulacion-2025-20/units/unit1/) se ensena las diferentes formas de distribucion dibujando puntos en una posicion x horizontal

1. Arriba: distribución uniforme
2. Centro: distribución normal con desviación pequeña
3. Abajo: distribución normal con desviación mayor

<img width="176" height="170" alt="image" src="https://github.com/user-attachments/assets/01c2d2e4-fb31-48c2-9714-e8f30c3ed4b1" />

Campana de Gauss

Son conceptos que hemos venido aprendiendo en procesos estocasticos la desviacion estanddar es una medida de que tanto se dsipersan los datos desde la media si la desviacion es pequena por ejemplo 1 entonces todos se tienden a pegar al centro, si la desviacion es grande por ejemplo 10 se dispersan mas hacia lo ancho (hacia los lados) 

<img width="300" alt="image" src="https://github.com/user-attachments/assets/61bf9185-a376-44e0-97b2-8c0ccb722436" />

<details>
  <summary>Distribucion Uniforme</summary>

```js
let x = random(100); // Entre 0 y 100, todos los valores con igual probabilidad
let y = 25;
circle(x, y, 5);
```
- El punto puede caer en cualquier parte del eje X.

- Como todos los valores tienen la misma probabilidad, la línea se verá uniformemente poblada.

</details>

<details>
  <summary>Distribución normal con poca desviación</summary>

```js
x = randomGaussian(50); // media = 50, sd = 1 (por defecto) desv. estand.
y = 50;
circle(x, y, 5);
```
- La mayoría de los puntos se agrupan muy cerca de 50.

- Rara vez se alejan mucho.

- Resultado: una línea muy concentrada en el centro (casi como un punto gordo)

</details>

<details>
  <summary>Distribución normal con mayor desviación</summary>

```js
x = randomGaussian(50, 10); // media = 50, sd = 10 desv. estand.
y = 75;
circle(x, y, 5);
```
- También se agrupan cerca de 50, pero ahora se dispersan más.

- A veces aparecen puntos más alejados, pero siguen siendo más frecuentes los cercanos a 50.

- Resultado: una línea con forma de campana
</details>

Entonces... cual es la diferencia entre una distribucion uniforme y una no uniforme de num aleatorios:

La diferencia está en la probabilidad con la que aparecen los valores.
En una distribución uniforme, todos los valores tienen la misma probabilidad de ocurrir (como lanzar un dado certificado: cada cara tiene 1/6 de probabilidad) En cambio, en una distribución no uniforme, algunos valores tienen más probabilidad que otros. Esto permite controlar el comportamiento aleatorio, como favorecer que un lado del dado tenga mas probabilidad de caer sobre una cara que sobre las otras (dados cargados)


#### Caminata aleatoria modificada con distribucion de probabildiades, distribucion de desviaciones 

En el codigo del Random Walker queremos modificar la direccion de movimiento ya que como esta construida tiene la misma probabildiad de movimiento en cualquier direccion:

```js
const choice = floor(random(4));
```
0 → derecha

1 → izquierda

2 → abajo

3 → arriba

Lo cambiaremos por algo no uniforme que favorezca is a la derecha

Mi primer acercamiento fue el de simplemente modificar 

```js
step() {
  const choice = floor(random(4));
  if (choice == 0) {
    this.x++;
  } else if (choice == 1) {
    this.x-2; // Esta línea mueve al walker a la derecha
  } else if (choice == 2) {
    this.y++; 
  } else {
    this.y--;
  }
}

```

pero este acercameinto no controla la distribucion de probabilidad sino que cambia la magnitud del paso hacia la derecha es decir que tan lejos del pixel actual se va a mover por asi decirlo.. por lo que es mejor establecerle porcentajes a cada una de las direcciones y darle un peso de proabilidad mas pesado al de la derecha:

```js
  step() {
    let r = random(1);
    if (r < 0.5) {
      this.x++; // 50% derecha
    } else if (r < 0.65) {
      this.x--; // 15% izquierda
    } else if (r < 0.825) {
      this.y++; // 17.5% abajo
    } else {
      this.y--; // 17.5% arriba
    }
  }
```

De esta manera el walker tiende a ir hacia la derecha como un dado cargado cayendo con mas probabilidad sobre un lado 

<details>
  <summary>Codigo Walker aleatorio modificado con distribucion No uniforme</summary>

#### Random Walker no uniforme

```js
// The Nature of Code
// Daniel Shiffman
// Modificado (Caminata aleatoria con tonos azules)

let walker;

function setup() {
  createCanvas(340, 640);
  walker = new Walker();
  background(0); // Fondo oscuro para resaltar el azul
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    let r = random(0, 30); // rojo
    let g = random(0, 50);  // verde
    let b = random(150, 255);  // azul
    stroke(r,g,b);
    fill(100); // con esto relleno el circulo de gris
    circle(this.x, this.y,15);
  }

  step() {
    let r = random(1);
    if (r < 0.5) {
      this.x++; // 50% derecha
    } else if (r < 0.65) {
      this.x--; // 15% izquierda
    } else if (r < 0.825) {
      this.y++; // 17.5% abajo
    } else {
      this.y--; // 17.5% arriba
    }
  }
}

```

</details>

### Actividad 5

la distribucion normaal o gaussiana es una forma de representar datos donde la mayoria de los valores se agrupan cerca del promedio(la media) y pocos valores se alejan mucho de el.... visualmente se ve como una curva en forma de campanaa (bell curve) la media es el valor mas probabile y la desviacion estandar determina que tan dispersos estan los valores con respectos a la media

```js
// Distribución normal como histograma

let histogram = [];
let totalBins = 100;

function setup() {
  createCanvas(800, 400);
  for (let i = 0; i < totalBins; i++) {
    histogram[i] = 0;
  }
  background(255);
}

function draw() {
  // Generar un valor con distribución normal centrado en el medio del canvas
  let num = floor(randomGaussian() * 15 + totalBins / 2);
  
  if (num >= 0 && num < totalBins) {
    histogram[num]++;
  }

  // Visualización
  background(255);
  stroke(0);
  fill(100, 100, 255, 150);

  let w = width / totalBins;

  for (let i = 0; i < histogram.length; i++) {
    let h = histogram[i] * 2;
    rect(i * w, height - h, w - 1, h);
  }
}
```

[Codigo de Distribucion Normal p5js](https://editor.p5js.org/DanielZafiro/sketches/hws1Q6URz)

<img width="600" alt="image" src="https://github.com/user-attachments/assets/30a0ae8e-7385-4a76-9555-7247fddfb9a0" />

### Actividad 06

> [!NOTE]
>
>  Bitácora
> Crea un nuevo sketch en p5.js donde modifiques uno de los ejemplos anteriores y adiciones de Lévy flight.
> Explica por qué usaste esta técnica y qué resultados esberabas obtener.
> Copia el código en tu bitácora.
> Coloca en enlace a tu sketch en p5.js en tu bitácora.
> Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

Recordando el ejemplo del tiburon que esta nadando con calma junto a cardumen y de la nada se lanza al ataque para luego volver a nadar en calma y este comportamiento hacerlo de manera aleatoria

Un Lévy flight es un tipo de movimiento aleatorio donde la mayoría de los pasos son cortos, pero ocasionalmente aparecen saltos largos. A diferencia de la caminata aleatoria tradicional (donde todos los pasos tienen una longitud similar), el Lévy flight usa una distribución de potencia o una distribución personalizada que favorece pasos pequeños pero no descarta los grandes.

Este comportamiento aparece en la naturaleza: por ejemplo, en la forma en que los animales exploran en busca de comida o cómo navega un ojo humano al mirar una imagen.

```js
let walker;

function setup() {
  createCanvas(800, 400);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.display();
}

class Walker {
  constructor() {
    this.pos = createVector(width / 2, height / 2);
  }

  step() {
    // Dirección aleatoria
    let step = p5.Vector.random2D();

    // Paso tipo Lévy, pero con un límite razonable para evitar valores extremos
    let r = constrain(pow(random(1), -1.5), 0, 20);  // Saltos de máximo 20
    step.mult(r); 

    this.pos.add(step);

    // Limitar al canvas
    this.pos.x = constrain(this.pos.x, 0, width);
    this.pos.y = constrain(this.pos.y, 0, height);
  }

  display() {
    stroke(0, 50);  // Hacemos más visible el trazo
    strokeWeight(2); // Aumentamos grosor del punto
    point(this.pos.x, this.pos.y);
  }
}

```

Con esta tecnica se puede evidenciar como el comportamiento del walker aleatorio pega esos saltos que pueden brindar aleatoriedad y sorpresa a la hora de estar a la espectativa de un cambio 

[Codigo p5js](https://editor.p5js.org/)


<img width="300" alt="image" src="https://github.com/user-attachments/assets/29438a08-52c0-4cde-b8d3-66b7d70149de" />


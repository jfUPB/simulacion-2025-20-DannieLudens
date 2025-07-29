# Unidad 1

## 🛠 Fase: Apply

### Actividad 8

<details>
  <summary>Enunciado: Creacion de obra generativa</summary>

Vas a crear una obra generativa interactiva en tiempo real utilizando los conceptos de aleatoriedad que has aprendido en esta unidad.
Tu obra debe:

- Usar al menos tres conceptos estudiados en esta unidad COMBINADOS de manera creativa y coherente.
- Tu obra de ser interactiva y generativa en tiempo real. Puedes usar el mouse, el teclado o cualquier otro sensor de entrada para interactuar con la obra.

#### Consolidación y metacognición 🤔
Ahora que has experimentado con la aleatoriedad y has aplicado estos conceptos en una pieza de arte generativo, es momento de reflexionar sobre el proceso y los resultados obtenidos.

🚧 Esta parte de la unidad la realizarás en la sesión 2 de la semana entrante.

</details>

> [!NOTE]
> Reporta en tu bitácora lo siguiente:
>
> 1. Un texto donde expliques el concepto de obra generativa.
> 2. Copia el código en tu bitácora.
> 3. Coloca el enlace a tu sketch en p5.js en tu bitácora.
> 4. Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.


<img src="https://github.com/user-attachments/assets/7e23ceba-cf53-4740-b2fd-e04366bbe522" width="400">


Inspiracion basada en el comportamiento de los cardumen de peces cuando se estan alimentando y cuando tienen cerca un depredador o un pez mas grande.

   Un conjunto de peces conforman el cardumen que actua como un perlin noise walker, hay un pez mas grande que podria ser un depredador, una tortuga, un delfin, si el pez grande se acerca al cardumen estos peces van a tener un comportamiento de evasion como se van a empezar a mover en una direccion aleatoria alejandose del pez grande, para que sus movimientos parezcan organicos usar el noise 2D de perlin funciona excelente por la suavidad del movimiento.

   además con el click del mouse se les pondrá alimento, tambien el alimento cae de manera aleatoria usando la distribucion no uniforme, la idea seria que los peces se acercaran a la particula mas cercana para alimentarse y aumentar su tamaño (radio) y alejarse del pez grande y que el pez grande fuera a cazar al pez mas grande con flight Levy.

<details>
  <summary>Concepto obra generativa Pecera</summary>

   Verificacion de cumplimiento de requisitos, almenos 3 conceptos estudiados combinados:
   
- [x] Aleatoriedad
- [x] Ruido perlin (noise 2D)
- [x] Random walker
- [x] Distribucion no uniforme
- [x] Minimo 3 conceptos combinados
- [x] Coherencia y creatividad
- [x] Interactiva y generativa en tiempo real
- [x] Elemento fisico interactivo (mouse o teclado)


3. Codigo del programa

<details>
  <summary>index.html</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.7.0/p5.min.js"></script>
    <meta charset="utf-8" />
    <link rel="stylesheet" type="text/css" href="style.css" />
  </head>
  <body>
    <script src="sketch.js"></script>
  </body>
</html>
```
</details>

<details>
  <summary>sketch.js</summary>

```js
let peces = [];  
let comidas = [];
let numPeces = 40; 
let bigFish; 
let distanciaHuida = 50; 
let margin = 20; 

function setup() {
  createCanvas(800, 600);  
  for (let i = 0; i < numPeces; i++) {
    peces.push(new Pez(random(width), random(height)));
  }

  bigFish = new BigFish(width / 2, height / 2);
}

function draw() {
  background(30, 144, 255);

  bigFish.update(peces); // objeto.metodo(parametros) receives the list of small fish 
  bigFish.display();

  for (let i = comidas.length - 1; i >= 0; i--) {
    comidas[i].display();
  }

  if (frameCount % 120 === 0) {
    let stdDevX = width / 4;
    let stdDevY = height / 4;
    let x = constrain(randomGaussian(width / 2, stdDevX), margin, width - margin);
    let y = constrain(randomGaussian(height / 2, stdDevY), margin, height - margin);
    comidas.push(new Comida(x, y));
  }

  for (let i = peces.length - 1; i >= 0; i--) {
    let p = peces[i];
    p.update(bigFish, comidas);
    p.display();

    if (
      dist(bigFish.pos.x, bigFish.pos.y, p.pos.x, p.pos.y) <
        bigFish.tamano / 2 + p.tamano / 2 &&
      !bigFish.cooldownActivo
    ) {
      peces.splice(i, 1);
      bigFish.iniciarCooldown(20);
    }
  }
}

function mousePressed() {
  let x = constrain(mouseX, margin, width - margin);
  let y = constrain(mouseY, margin, height - margin);
  comidas.push(new Comida(x, y));
}

class Pez {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.tamano = 8;
    this.noiseOffset = createVector(random(1000), random(1000));
    this.vel = createVector();
  }

  update(bigFish, comidas) {
    let d = dist(this.pos.x, this.pos.y, bigFish.pos.x, bigFish.pos.y);

    let nx = noise(this.noiseOffset.x) * TWO_PI * 2;
    let ny = noise(this.noiseOffset.y) * TWO_PI * 2;
    this.vel = createVector(cos(nx), sin(ny)).mult(1.5);
    this.noiseOffset.add(0.01, 0.01);

    let escape = createVector();                   
    if (d < distanciaHuida) {
      escape.add(
        p5.Vector.sub(this.pos, bigFish.pos).setMag(2)
      );
    }

    if (this.pos.x < margin)       escape.add(createVector(1, 0));
    else if (this.pos.x > width - margin) escape.add(createVector(-1, 0));
    if (this.pos.y < margin)       escape.add(createVector(0, 1));
    else if (this.pos.y > height - margin) escape.add(createVector(0, -1));
    escape.setMag(escape.mag() ? 2 : 0);

    this.pos.add(this.vel).add(escape);

    if (comidas.length > 0 && d > distanciaHuida) {
      let objetivo = this.comidaMasCercana(comidas);
      let dir = p5.Vector.sub(objetivo.pos, this.pos).setMag(0.5);
      this.pos.add(dir);
      if (
        dist(this.pos.x, this.pos.y, objetivo.pos.x, objetivo.pos.y) <
        (this.tamano + objetivo.tamano) / 2
      ) {
        this.tamano += 1;
        comidas.splice(comidas.indexOf(objetivo), 1); 
      }
    }

    this.pos.x = constrain(this.pos.x, 0, width);
    this.pos.y = constrain(this.pos.y, 0, height);
  }

  comidaMasCercana(lista) {
    let mejor = null;
    let minD = Infinity;
    for (let c of lista) {
      let d = dist(this.pos.x, this.pos.y, c.pos.x, c.pos.y);
      if (d < minD) { minD = d; mejor = c; }
    }
    return mejor;
  }

  display() {
    fill(255, 200);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.tamano);
  }
}

class BigFish {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.tamano = 30;
    this.noiseOffset = createVector(random(5000), random(5000));
    this.vel = createVector();
    this.target = null;
    this.cooldownActivo = false;
    this.tiempoRestante = 0;
    this.tiempoFin = 0;
  }

  iniciarCooldown(segundos) {
    this.cooldownActivo = true;
    this.tiempoRestante = segundos;
    this.tiempoFin = millis() + segundos * 1000;
  }

  update(peces) {
    let angleNoise = noise(this.noiseOffset.x, this.noiseOffset.y) * TWO_PI * 2;
    let perlinDir = p5.Vector.fromAngle(angleNoise).mult(1.5);

    if (this.cooldownActivo) {
      this.target = null;
      this.vel = perlinDir;
      this.tiempoRestante = max(0, (this.tiempoFin - millis()) / 1000);
      if (this.tiempoRestante <= 0) {
        this.cooldownActivo = false;
        this.tiempoRestante = 0;
      }

    } else if (peces.length > 0) {
      let mayor = peces.reduce((a, b) => (a.tamano > b.tamano ? a : b));
      this.target = mayor;
      let chase = p5.Vector.sub(mayor.pos, this.pos).normalize().mult(2.5);
      this.vel = p5.Vector.add(perlinDir, chase).limit(3);

    } else {
      this.vel = perlinDir;
      this.target = null;
    }

    this.pos.add(this.vel);
    this.noiseOffset.add(0.01, 0.01);

    this.pos.x = constrain(this.pos.x, 0, width);
    this.pos.y = constrain(this.pos.y, 0, height);
  }

  display() {
    fill(0, 255, 0, 180);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.tamano);

    if (this.cooldownActivo) {
      fill(255);
      textAlign(CENTER, CENTER);
      textSize(14);
      text(nf(this.tiempoRestante, 1, 1), this.pos.x, this.pos.y - this.tamano);
    }

    if (this.target) {
      noFill();
      stroke(255, 0, 0);
      strokeWeight(2);
      ellipse(this.target.pos.x, this.target.pos.y, this.target.tamano + 6);
    }
  }
}

class Comida {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.tamano = 8;
  }

  display() {
    fill(255, 150, 0);
    noStroke();
    rectMode(CENTER);
    rect(this.pos.x, this.pos.y, this.tamano, this.tamano);
  }

}
```
</details>

3. [Enlace al codigo en p5js sobre la obra generativa: titulo por definir](https://editor.p5js.org/DanielZafiro/sketches/6Q7fMKdSM)


</details>

# Unidad 1

## 🤔 Fase: Reflect

### Actividd 9

#### Parte 1: recuperación de conocimiento (Retrieval Practice)

1. Diferencia fundamental entre la aleatoriedad generada por random() y la apariencia de aleatoriedad del ruido perlin Noise() en que tipo de situacion usarias cada una

**Respuesta:** La diferenencia fundamental entre estas dos es que para implementar el random es necesario restringir el rango de valores que se quieren aleatorias mientras que el ruido perlin es generado y aunque tambien se puede restringir su rango es mas utilizado para generar aleatoriedad que agarrar un numero aleatorio de la bolsa de numeros (por asi decirlo)

El Ruido perlin se puede usar para generar texturas por ejemplo en un objeto de blender, tambien se puede usar para generar noise para luego modular una onda y generar un sonido, tambien se puede usar para generar terrenos y oleaje, tambien para suavisar la curva de un conjunto de datos distribuidos no uniformes o uniformes

El Random se puede usar para elegir un rango de colores, tambien para elegir un numero entre muchos, pensandolo en proyectos seria elegir aleatoriamente un dato que puede estar dentro de un arreglo de manera aleatoria y tambien para elegir posiciones aleatorias

2. Con mis palabras, que es una distribucion de probabilidad y cual es la diferencia visual que produce uan caminata aleatoria con una distribucion unifrme y una con distribucion normal

**Respuesta:** La distribucion de probabilidad es cómo estan distribuidos los datos de manera que un dato es mas probable que otro con respecto a la media pero tambien dependen de que tipo de distribucion

la diferencia visual de un random walker con distribucion uniforme a otro con distribucion normal esque el walker en la normal se va a ver mucho mas concentrado caminando casi que en el mismo punto mientras que el walker uniforme podria tener ciertas tendencias a moverse en un espacio mas amplio pero concentrado

3. Cual es el papel de la aleatoriedad en el arte generativo

**Respuesta:** generar experiencias y comportamientos distintos cada vez que se experimente con la obra, genera sostenimiento de la obra en el tiempo ya que cada vez que se visite sera algo similar pero unico, asi como en la naturaleza permite diversidad (analogia del crecimiento de un arbol segun el entorno)

4. el concepto de la comida cayendo sobre la pecera utiliza la distribucion no uniforme lo que hace que funcione como si actuaran factores externos como el viento, la corriente de aire que empujo un granito de comida y asi, tambien el comportamiento de los peces cuando el pez controlado con el mouse que bien puede alejarse tranquilos o puede que se asusten y salgan disparados utilizando la el vuelo de Levy

5. Que es un paseo o caminata en el contexto de la simulacion, que caracteristica particular tiene una caminata de tipo Lvey flight

**Respuesta:** un walk en simulacion es un movimiento simulando una caminata en este contexto de aleatoriedad implica que el walker se movera de manera aleatoria en una de 4 direcciones (hasta mas) y la caractersitica principal que tiene la caminata con Levy esque hace pasos cortos y en algun momento puede hacer un paso grande y esto ocurrir de manera aleatoria

#### Parte 2: reflexión sobre tu proceso (Metacognición)

1. Cual fue el concepto mas abstracto o dificil de visualizar para mi en esta unidad y que hice para finalmente comprenderlo

**Respuesta:** La diferernciacion entre las distribuciones uniformes y distribucion normal lo que hice fue consultar porque me genera cierta confusion al ser muy parecidas:

La distribución uniforme asigna la misma probabilidad a todos los valores dentro de un rango específico, mientras que la distribución normal, también conocida como distribución de campana, concentra la probabilidad alrededor de la media, con valores más alejados de la media teniendo menor probabilidad. 

2. Describe un momento durante el desarrollo de tu obra final (Actividad 08) en el que un “error” o un resultado inesperado te llevó a una idea creativa interesante.

**Respuesta:** El error que encontré esque el comportamiento de los peces del cardumen no era el esperado con el random walker pues parecian temblando todoe el tiempo. tambíen lo que pretendia era que los peces se alejaran en direccion contraria de manera aleatoria en direccion al pez principal si se acercaba, lo que me represento un reto porque tenia que repensar o diseñar un comportamiento para los peces dependiendo de la posición del pez principal para que se movieran en una direccion diferente con tendencia aleatoria fuera del rango del pez grande, lo que me llevó a implementar

3. Al combinar diferentes técnicas de aleatoriedad, ¿Cuál fue el mayor desafío? ¿La interacción entre los sistemas, el control de los parámetros, el rendimiento?

**Respuesta:** Siento que la interaccion entre los sistemas en el sketch inicial no era el mejor ya que por un lado caia la comida de los peces del cielo con distribucion no uniforme pero los peces no interactuaban con la comida entonces se sentia desligado con la obra y parecia mas lluvia... el otro elemento que no me cuadraba era que habia diseñado la obra de tal manera que el usuario controlara con el mouse el pez grande para perseguir a los peces pequeños pero no se sentia natural y los conceptos de aleatoriedad quedaban en segundo plano... finalmente con los cambios que hice la obra se siente mas viva


4. Si tuvieras que empezar la Actividad 08 de nuevo, ¿Qué harías de manera diferente basándote en lo que sabes ahora?

**Respuesta:** combinaria mejor la aleatoriedad es decir que los peces fueran walkers aleatorios pero mas suavizado cuando el pez principal se acercara su comportamiento fuera de un walker levy flight y que los peces del cardumen nadaran hacia la comida que esta mas cerca de cada uno y lo consumiera volviendose un pez mas grande cada vez (nueva implementación en el apply)


Obra antes:

<details>
   <summary>sketch</summary>

```js
let mainFish;
let fishSchool = [];
let food = [];

function setup() {
  createCanvas(800, 600);
  mainFish = new Fish(mouseX, mouseY, true);

  // Crear el cardumen
  for (let i = 0; i < 30; i++) {
    fishSchool.push(new Fish(random(width), random(height)));
  }
}

function draw() {
  background(30, 144, 255); // Azul pescera

  // Dibujar y mover el pez principal
  mainFish.x = mouseX;
  mainFish.y = mouseY;
  mainFish.display();

  // Dibujar y mover el cardumen
  for (let f of fishSchool) {
    f.update(mainFish);
    f.display();
  }

  // Generar comida distri no uniforme
  if (frameCount % 20 == 0) {
    let rx = width / 2 + randomGaussian() * 100; // centrado pantalla, distribución normal
    food.push(new Food(rx, 0));
  }

  // Dibujar y mover la comida
  for (let i = food.length - 1; i >= 0; i--) {
    food[i].update();
    food[i].display();
    if (food[i].offscreen()) {
      food.splice(i, 1);
    }
  }
}

// Clase Pez
class Fish {
  constructor(x, y, isMain = false) {
    this.x = x;
    this.y = y;
    this.isMain = isMain;
    this.radius = isMain ? 16 : 10;
  }

  update(target) {
    if (!this.isMain) {
      let d = dist(this.x, this.y, target.x, target.y);
      if (d < 100) {
        // alejarse aleatoriamente
        this.x += random(-5, 5);
        this.y += random(-5, 5);
      } else {
        // reagruparse con movimiento aleatorio suave
        this.x += random(-1, 1);
        this.y += random(-1, 1);
      }
    }
  }

  display() {
    fill(this.isMain ? 'yellow' : 'white');
    noStroke();
    ellipse(this.x, this.y, this.radius * 2);
  }
}

// Clase Comida
class Food {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.speed = random(1, 3);
  }

  update() {
    this.y += this.speed;
  }

  display() {
    fill('orange');
    noStroke();
    rect(this.x, this.y, 6, 6);
  }

  offscreen() {
    return this.y > height;
  }
}
```

</details>

<img src="https://github.com/user-attachments/assets/d3b7e118-da33-43e5-93ea-e64e955e40c3" width="400">

[link p5js pecera_v1](https://editor.p5js.org/DanielZafiro/sketches/NqZ-8oHE6)

Obra despues:

<details>
   <summary>sketch</summary>

```js

let peces = [];  // Arreglo que contendrá los objetos Pez (cardumen)
let comidas = []; // Arreglo que contendrá objetos Comida
let numPeces = 40; // Número de peces iniciales en el cardumen
let bigFish; // Variable para el pez depredador
let distanciaHuida = 50; // Distancia mínima a la que un pez huye del depredador
let margin = 20; // Margen seguro para generar comida y evitar bordes inaccesibles

function setup() {
  createCanvas(800, 600);   // Crear un canvas de 800×600 píxeles
  //createCanvas(windowWidth,windowHeight); // Canvas segun el espacio disponible en el preview
  
  // Inicializar el cardumen con posiciones aleatorias
  for (let i = 0; i < numPeces; i++) {
    peces.push(new Pez(random(width), random(height)));
  }
  // Crear el pez depredador en el centro del canvas
  bigFish = new BigFish(width / 2, height / 2);
}

function draw() {
  // Fondo color azul agua
  background(30, 144, 255);

  // 1) Actualizar y mostrar el pez depredador
  bigFish.update(peces);
  bigFish.display();

  // 2) Mostrar toda la comida existente
  for (let i = comidas.length - 1; i >= 0; i--) {
    comidas[i].display();
    // No se elimina automáticamente en offScreen aquí, offScreen siempre devuelve false
  }

  // 3) Generación autónoma de comida cada 2 segundos (120 frames)
  if (frameCount % 120 === 0) {
    // Desviaciones estándar para x e y
    let stdDevX = width / 4;
    let stdDevY = height / 4;
    // Posición x centrada con distribución normal, limitada por margin
    let x = constrain(randomGaussian(width / 2, stdDevX), margin, width - margin);
    // Posición y centrada con distribución normal, limitada por margin
    let y = constrain(randomGaussian(height / 2, stdDevY), margin, height - margin);
    comidas.push(new Comida(x, y));
  }

  // 4) Actualizar y mostrar cada pez del cardumen
  for (let i = peces.length - 1; i >= 0; i--) {
    let p = peces[i];
    p.update(bigFish, comidas);
    p.display();

    // Si el depredador come al pez y no está en cooldown...
    if (
      dist(bigFish.pos.x, bigFish.pos.y, p.pos.x, p.pos.y) <
        bigFish.tamano / 2 + p.tamano / 2 &&
      !bigFish.cooldownActivo
    ) {
      peces.splice(i, 1);            // Elimina el pez
      bigFish.iniciarCooldown(20);  // Inicia cooldown de 20s
    }
  }
}

function mousePressed() {
  // Al hacer click, generar comida manual en la posición del mouse
  let x = constrain(mouseX, margin, width - margin);
  let y = constrain(mouseY, margin, height - margin);
  comidas.push(new Comida(x, y));
}

// ---------------------------
// Clase Pez (cardumen)
// ---------------------------
class Pez {
  constructor(x, y) {
    this.pos = createVector(x, y);                    // Vector de posición
    this.tamano = 8;                                   // Radio del pez
    this.noiseOffset = createVector(random(1000), random(1000)); // Offset para Perlin noise
    this.vel = createVector();                         // Velocidad actual
  }

  update(bigFish, comidas) {
    // Distancia al pez depredador
    let d = dist(this.pos.x, this.pos.y, bigFish.pos.x, bigFish.pos.y);

    // 1) Movimiento base con Perlin noise
    let nx = noise(this.noiseOffset.x) * TWO_PI * 2;
    let ny = noise(this.noiseOffset.y) * TWO_PI * 2;
    this.vel = createVector(cos(nx), sin(ny)).mult(1.5);
    this.noiseOffset.add(0.01, 0.01); // Avanza el ruido en el tiempo

    // 2) Fuerza de escape
    let escape = createVector();                   
    if (d < distanciaHuida) {
      // Huye del depredador si está dentro de distanciaHuida
      escape.add(
        p5.Vector.sub(this.pos, bigFish.pos).setMag(2)
      );
    }

    // 3) Evitar bordes dentro de margin
    if (this.pos.x < margin)       escape.add(createVector(1, 0));
    else if (this.pos.x > width - margin) escape.add(createVector(-1, 0));
    if (this.pos.y < margin)       escape.add(createVector(0, 1));
    else if (this.pos.y > height - margin) escape.add(createVector(0, -1));
    escape.setMag(escape.mag() ? 2 : 0);

    // 4) Aplica movimiento y escape
    this.pos.add(this.vel).add(escape);

    // 5) Búsqueda y consumo de comida si no está huyendo
    if (comidas.length > 0 && d > distanciaHuida) {
      let objetivo = this.comidaMasCercana(comidas);
      let dir = p5.Vector.sub(objetivo.pos, this.pos).setMag(0.5);
      this.pos.add(dir);
      if (
        dist(this.pos.x, this.pos.y, objetivo.pos.x, objetivo.pos.y) <
        (this.tamano + objetivo.tamano) / 2
      ) {
        this.tamano += 1;                           // Aumenta tamaño al comer
        comidas.splice(comidas.indexOf(objetivo), 1); // Elimina comida
      }
    }

    // 6) Mantiene dentro del canvas
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

// ---------------------------
// Clase BigFish (depredador)
// ---------------------------
class BigFish {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.tamano = 30;
    this.noiseOffset = createVector(random(5000), random(5000));
    this.vel = createVector();
    this.target = null;     // Presa actual
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
    // 1) Genera vector Perlin noise
    let angleNoise = noise(this.noiseOffset.x, this.noiseOffset.y) * TWO_PI * 2;
    let perlinDir = p5.Vector.fromAngle(angleNoise).mult(1.5);

    // 2) Si está en cooldown: solo ruido y conteo regresivo
    if (this.cooldownActivo) {
      this.target = null;
      this.vel = perlinDir;
      this.tiempoRestante = max(0, (this.tiempoFin - millis()) / 1000);
      if (this.tiempoRestante <= 0) {
        this.cooldownActivo = false;
        this.tiempoRestante = 0;
      }

    // 3) Si hay peces: buscar al más grande y cazarlo
    } else if (peces.length > 0) {
      let mayor = peces.reduce((a, b) => (a.tamano > b.tamano ? a : b));
      this.target = mayor;
      let chase = p5.Vector.sub(mayor.pos, this.pos).normalize().mult(2.5);
      this.vel = p5.Vector.add(perlinDir, chase).limit(3);

    // 4) Si no hay nada: solo ruido Perlin
    } else {
      this.vel = perlinDir;
      this.target = null;
    }

    // 5) Aplica movimiento y avanza el ruido
    this.pos.add(this.vel);
    this.noiseOffset.add(0.01, 0.01);

    // 6) Limitar al canvas
    this.pos.x = constrain(this.pos.x, 0, width);
    this.pos.y = constrain(this.pos.y, 0, height);
  }

  display() {
    fill(0, 255, 0, 180);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.tamano);

    // Mostrar texto de cooldown si aplica
    if (this.cooldownActivo) {
      fill(255);
      textAlign(CENTER, CENTER);
      textSize(14);
      text(nf(this.tiempoRestante, 1, 1), this.pos.x, this.pos.y - this.tamano);
    }

    // Dibujar indicador sobre la presa
    if (this.target) {
      noFill();
      stroke(255, 0, 0);
      strokeWeight(2);
      ellipse(this.target.pos.x, this.target.pos.y, this.target.tamano + 6);
    }
  }
}

// ---------------------------
// Clase Comida
// ---------------------------
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


<img src="https://github.com/user-attachments/assets/a8387405-ede8-4b18-8105-cb2b853b1d58" width="400">

[link p5js pecera_v2](https://editor.p5js.org/DanielZafiro/sketches/6Q7fMKdSM)

### Actividad 10

[Bitacora de Parra](https://github.com/jfUPB/simulacion-2025-20-Nilexpat/blob/unidad1/reflect/unidad-1/reflect.md )

Giff de su obra generativa:

<img src="https://github.com/user-attachments/assets/dcc2a45a-4135-43a3-8c75-2d559df61898" width="400">


#### Comentario de retroaliemntacion constructiva para la Act8 de Parra

Su Obra generativa sobre fuegos artificiales mediante la implementacion de particulas que se dispersan de manera aleatoria desde el centro me parecio muy chevere, no solo por que aplico conceptos de probabilidad para el cambio de color o la distribucion de las particulas sino porque logró encapsular en un mismo concepto estos elementos para que el espectador tuviese una experiencia visual unica cada vez

El feedback que le dí fue que le podria agregar el concepto de distribucion no uniforme a la aparicion del fuego artificial para que no siempre fuera en el centro sino en un rango aleatorio horizontal y adicional que las particulas mientras se disiparan no se borraran inmeditamente al dar click para generar un nuevo fuego artificial sino que tambien se fueran desapareciendo lentamente de esta manera darle continuidad a la obra conservando los fuegos artificales anteriores



### Actividad 11

1. **Continuar:** Los ejemplos alternos a los mostrados en el libro para entender mejor como se usan esos conceptos en otros espacios, ayuda a mejorar la perpectiva y conectar con experiencias clave que podemos identificar, esto deberia mantenerse si o si.

   Adicionalmente los plazos de entrega establecidos este semestre suponen un reto que le agrega a esta gran receta no solo un objetivo o meta sino tambien un limite y de una u otra manera ayuda a tener cierta presion que acelera ese instinto o deseo de cumplir de buena manera (no se porque mi cerebro trabaja mejor bajo presion es como si me obligara a comprometerme más)

3. **Dejar de hacer:** Quizas lo unico que me parece que deberia dejar de hacerse en el curso seria dejar bloqueado la siguiente unidad mientras estamos en la unidad anterior por ejemplo en la unidad 1 llegamos al reflect y me gustaria trabajar en la unidad 2 para ir adelantando y no esperar al martes para empezar con el seek

    No hubo ningun Concepto que me pareciera redundate, confuso hasta el momento todo a sido muy claro y util

3. **Empezar a hacer:** Me hizo falta en la ultima clase ver las obras generativas de los compañeros tipo un showcase de todos, porque brinda panoramas de ideas de concepto que los demas han explorado

4. **Balance teoría-práctica:** Analizar los ejemplos del texto guia tanto de los ejemplos de la pagina del curso como de nature of code y la documentacion de p5js me han parecido muy utiles e ilustrativos sobre todo para entender los conceptos aplicados y visualmente la logica detras, siento que todavia en la practica me quedo mucho tiempo leyendo, modificando y haciendo preguntas a la IA sobre el funcionamiento de los ejemplos e ideas que se me van ocurriendo, lo cual no es util para las entregas agiles del curso

5. **Comentario adicional:** Por el momento me esta encantando la metodologia del curso, si bien empecé algo atrasado, es algo que no me estoy permitiendo más para mantener esa disciplina.

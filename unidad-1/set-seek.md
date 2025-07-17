# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1 

> [!NOTE]
> · Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo.


Así como ocurre en el universo y en la vida, la aleatoriedad es parte esencial de los acontecimientos que los hacen únicos y hermosos. Con observar el cielo tenemos para darnos cuenta: cada día es diferente, irrepetible, y aun así, reconocible. Hay patrones entre amaneceres y atardeceres, pero nunca veremos exactamente el mismo cielo. Del mismo modo, en el arte generativo, la aleatoriedad introduce belleza dentro del caos existente, permitiendo que surjan obras únicas e impredecibles, tan fascinantes como la naturaleza misma

### Actividad 2

> [!NOTE]
> · Luego de ver el trabajo de Sofía piensa y escribe en TUS PROPIAS palabras:
> 
> Cuál es el papel de la aleatoriedad en su obra.
> Según tu perfil profesional, cómo se aplica el concepto de aleatoriedad en el tipo de proyectos que desarrollas. Ilustra tu respuesta con ejemplos concretos.

#### El papel de la aleatoriedad en la obra de Sofi

La aleatoriedad en la obra de Sofía garantiza que cada ejecución de la pieza sea distinta, como si fuera una extensión viva de la canción. Los movimientos y cambios de tono y color siguen una trayectoria impredecible, pero no caótica; al contrario, se sienten orgánicos y controlados, como si fueran una extremidad natural de la música. Esta conexión entre sonido y visual refuerza la fuerza del ritmo y genera una sensación de unidad entre lo visual y lo auditivo. Gracias a esta imprevisibilidad, el espectador se siente más conectado con la obra, ya que cada visualización es única e irrepetible.

#### La aleatoriedad en mi perfil profesional

Como futuro Ingeniero en Diseño de Entretenimiento Digital, considero que la aleatoriedad es una herramienta poderosa para generar dinamismo, inmersión y rejugabilidad. Un ejemplo claro lo viví en la materia Sistemas Físicos 1, donde implementé un sistema con Micro:bit que generaba un código aleatorio para desactivar una bomba virtual en p5.js. Esta aleatoriedad hacía que cada partida fuera similar, pero nunca igual, lo que aumentaba la emoción y el desafío.

Otro ejemplo lo viví en Sistemas Físicos Interactivos 2, donde utilicé la aleatoriedad para generar patrones visuales, palabras y formas que componían una obra diferente en cada ejecución. Esto no solo hacía que la pieza fuera más atractiva visualmente, sino que también le daba al usuario una sensación de participación activa en la creación del resultado final.

En ambos casos, la aleatoriedad no fue un elemento decorativo, sino una estrategia de diseño interactivo que aumentó el valor de la experiencia. Para mí, aplicar la aleatoriedad significa diseñar sistemas vivos, donde cada ejecución es una oportunidad nueva para descubrir algo distinto.

### Actividad 3 

#### Realiza el siguiente experimento y reporta los resultados en tu bitácora:

1. Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.

   La modificacón que realicé fue la de que cada vez que  el walker caminara se pintara el rastro de un tono aleatorio de azul y además que en vez de ser un punto fuera un circulo para que no se viera tan pequeño (aunque afecta la forma dado que el wlaker esta configurado para moverse un pixel a la vez por lo que el patrón es mas demorado en formarse)
    El codigo que hice fue este:

´´´js
// The Nature of Code
// Daniel Shiffman
// Modificado por Daniel (Caminata aleatoria con tonos azules)

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
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--; // -1  plano cartesiano desde el centro
    } else if (choice == 2) {
      this.y++; // +1
    } else {
      this.y--;
    }
  }
}

´´´

3. Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.

4. Ejecuta el código y escribe en tu bitácora qué sucedió realmente.

5. Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?

### Actividad 4



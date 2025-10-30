# Unidad 5

## Introducción 📜

En esta unidad voy a explorar los sistemas de partículas. Los sistemas de partículas son conjuntos de partículas que interactúan entre sí y con su entorno. Estos sistemas son fundamentales en el mundo del arte y diseño generativo, ya que permiten crear composiciones complejas y dinámicas a partir de reglas simples.

> [!WARNING]
> Evidencias en bitácora
>
> - En la bitácora debes incluir las siguientes evidencias:
>
> - Las solicitudes de la actividad 2 (Investigación).
> - Las solicitudes de la actividad 3 (Aplicación).
> - Tu mismo vas a propoer una nota basada en la rúbrica y justificarás cada criterio de la rúbrica indicando qué evidencias presentes en la bitácora justifican la nota que propones.
> - Si tu bitácora no incluye la nota propuesta y la justificación tendrás una nota de 0.

## Set: ¿Qué aprenderás en esta unidad? 💡

En esta unidad vas a experimentar con aplicaciones interactivas que tendrán las siguientes características:

- Emitirán partículas que se moverán en el espacio.
- Las partículas estarán sujetas a fuerzas que las harán cambiar de dirección y velocidad.


Ten presente que estos sistemas se ejecutarán en tiempo real usando la CPU. Por lo tanto, deberás tener en cuenta las limitaciones de rendimiento y optimización al diseñar tus sistemas de partículas. Mejor dicho, no podrás tener miles de partículas en pantalla sin que el rendimiento se vea afectado. Para lograr esto necesitarás recurrir a la programación de la GPU usando shaders. Pero esto lo harás en la siguiente temporada.

### Actividad 01

Solo para antojarte un poco, te pediré que mires el trabajo de uno de los artistas generativos más reconocidos actualmente. Se trata de [Refik Anadol](https://refikanadol.com/)

## Seek: Investigación 🔎

En esta fase, repasarás los conceptos de herencia y polimorfismo que aplicarás para crear sistemas de partículas que se comporten y se vean de manera diferente. Adicionalmente, aplicarás fuerzas a las partículas para que se muevan e interactúan con el entorno.

### Actividad 02

**Revisa y repasa algunos conceptos**

Dale una mirada al [capítulo sobre sistemas de partículas](https://natureofcode.com/particles/) del texto guía del curso. Explora libremente, pero te pediré que revises especialmente los siguientes conceptos:

1. Revisa detalladamente el ejemplo 4.2: [an Array of Particles.](https://natureofcode.com/particles/#example-42-an-array-of-particles)
2. Analiza el ejemplo 4.4: a [System of Systems.](https://natureofcode.com/particles/#example-44-a-system-of-systems)
3. Analiza el ejemplo 4.5: [a Particle System with Inheritance and Polymorphism.](https://natureofcode.com/particles/#example-45-a-particle-system-with-inheritance-and-polymorphism)
4. Analiza el ejemplo 4.6: [a Particle System with Forces.](https://natureofcode.com/particles/#example-46-a-particle-system-with-forces)
5. Analiza el ejemplo 4.7:[ a Particle System with a Repeller.](https://natureofcode.com/particles/#example-47-a-particle-system-with-a-repeller)

🧐🧪✍️ En tu bitácora responde a esta pregunta para cada una de las simulaciones: ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

Además te pediré que hagas los siguientes experimentos y los reportes en tu bitácora:

1. Vas a modificar cada una de las simulaciones anteriores incluyen en cada una, al menos un concepto de las unidades anteriores, pero no repitas concepto, la idea es que repases al menos uno de cada unidad.
2. Vas a gestionar la creación y la desaparición de las partículas y la memoria.
3. Explica cómo lo hiciste (aunque es posible que la simulación ya lo haga, trata de identificarlo de nuevo y explicarlo con tus palabras). Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
4. Incluye un enlace a tu código en el editor de p5.js.
5. Incluye el código fuente de cada una de las simulaciones.
6. Captura de pantallas de cada una de las simulaciones con las imágenes que más te gusten como resultado de la ejecución de cada una de las simulaciones.

---

<details>
  <summary>Ejemplo 4.2 - Arreglo de Partículas </summary>

#### Ejemplo 4.2 - an Array of Particles

---

<details>
  <summary>🔍 Gestión de memoria</summary>

En este ejemplo observamos que:

**1. Creación de partículas:**
- Se usa `particles.push(new Particle(width/2, 20))` en cada frame
- Esto hace que el array crezca constantemente
- Cada partícula se crea en el centro superior del canvas

**2. Desaparición de partículas:**
- El método `splice(i, 1)` elimina partículas cuando `isDead()` retorna `true`
- Se recorre el array AL REVÉS (`i--`) para evitar saltos de índice
- Esto es crucial: si eliminas elementos mientras avanzas, algunos elementos se "saltan"

**3. Observción:**
- Sin el `splice()`, el array crecería infinitamente (60 partículas/segundo a 60 FPS = 3,600 partículas por minuto)
- Esto consumiría RAM progresivamente y bajaría los FPS drásticamente
- Es como una "limpieza automática" de memoria - solo mantiene partículas vivas

**Diagrama del ciclo de vida de una particula:**
```
[CREAR] → [ACTUALIZAR] → [¿Muerta?] → [ELIMINAR]
   ↑              ↓           ↓
   |          [Viva]     [splice()]
   └──────────────┘
```

**Código clave:**
```javascript
// Recorrer AL REVÉS es fundamental
for (let i = particles.length - 1; i >= 0; i--) {
  let p = particles[i];
  p.update();
  p.show();
  
  // Limpieza de memoria
  if (p.isDead()) {
    particles.splice(i, 1);  // Elimina del array
  }
}
```

**Observación importante:**
El `lifespan` disminuye 2 unidades por frame (`lifespan -= 2`), lo que significa que cada partícula vive aproximadamente 127 frames (255/2 ≈ 2.1 segundos a 60 FPS).

</details>

---

<details>
  <summary>🧪 Concepto aplicado: Ruido Perlin (Unidad 1)</summary>

**Ruido Perlin para movimiento orgánico**

**¿Por qué elegí este concepto?**

Quería que las partículas tuvieran un movimiento más natural y fluido, similar a humo o vapor flotando en el aire, en lugar de simplemente caer por gravedad de forma predecible.

**¿Cómo lo implementé?**

1. **Variables únicas por partícula:**
```javascript
this.xoff = random(1000);  // Offset único en eje X
this.yoff = random(1000);  // Offset único en eje Y
```
Cada partícula comienza en una posición aleatoria del "espacio de ruido Perlin"

2. **Generación de fuerzas orgánicas:**
```javascript
let noiseX = map(noise(this.xoff), 0, 1, -0.1, 0.1);
let noiseY = map(noise(this.yoff), 0, 1, -0.05, 0.05);
let noiseForce = createVector(noiseX, noiseY);
this.acceleration.add(noiseForce);
```

3. **"Caminar" por el espacio de ruido:**
```javascript
this.xoff += 0.01;  // Avance suave en X
this.yoff += 0.01;  // Avance suave en Y
```

**¿Por qué diferentes rangos en X e Y?**
- Más movimiento horizontal (`-0.1 a 0.1`) crea dispersión lateral
- Menos movimiento vertical (`-0.05 a 0.05`) mantiene las partículas flotando sin subir demasiado

**Comparación con random():**
- `random()`: Saltos bruscos, movimiento errático y artificial
- `noise()`: Cambios suaves, movimiento fluido y natural

</details>

---

<details>
  <summary>💻 Código completo</summary>

```javascript
let particles = [];

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255, 25); // Transparencia para efecto de rastro
  
  particles.push(new Particle(width / 2, 20));
  
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.update();
    p.show();
    
    if (p.isDead()) {
      particles.splice(i, 1);
    }
  }
  
  fill(0);
  noStroke();
  text(`Partículas: ${particles.length}`, 10, 20);
}

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0.5);
    this.acceleration = createVector(0, 0);
    this.lifespan = 255;
    
    // Ruido Perlin
    this.xoff = random(1000);
    this.yoff = random(1000);
  }
  
  update() {
    // Generar fuerzas desde ruido Perlin
    let noiseX = map(noise(this.xoff), 0, 1, -0.1, 0.1);
    let noiseY = map(noise(this.yoff), 0, 1, -0.05, 0.05);
    let noiseForce = createVector(noiseX, noiseY);
    
    this.acceleration.add(noiseForce);
    this.velocity.add(this.acceleration);
    this.velocity.limit(3);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    
    // Avanzar en el espacio de ruido
    this.xoff += 0.01;
    this.yoff += 0.01;
    
    this.lifespan -= 2;
  }
  
  show() {
    stroke(100, 150, 255, this.lifespan);
    strokeWeight(2);
    fill(150, 200, 255, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }
  
  isDead() {
    return this.lifespan < 0;
  }
}
```

**🔗 Enlace p5.js:** [Ver código en vivo](https://editor.p5js.org/DanieLudens/sketches/603sVXTDi)

</details>

---

📸 **Resultados visuales**

**visuales:**
- ✅ El efecto de rastro (`background(255, 25)`) crea líneas suaves que revelan las trayectorias
- ✅ Los colores azules (#96C8FF aproximadamente) sugieren agua o aire
- ✅ El movimiento es hipnótico y natural, no mecánico
- ✅ Las partículas se dispersan lateralmente mientras flotan, creando un patrón de "humo ascendente"

**Diferencias clave:**
- **Sin Perlin:** Las partículas caen en líneas casi rectas con pequeñas variaciones
- **Con Perlin:** Las partículas trazan curvas suaves, como si flotaran en corrientes de aire

</details>

</details>

|Original|Modificado|
|-----|-----|
|<img width="400" src="https://github.com/user-attachments/assets/e868d5d8-5495-4295-91e9-561cf48002d6">|<img width="400" src="https://github.com/user-attachments/assets/0d5a3814-883d-4f54-a78d-1945d691ccea">|


<details>
  <summary>Ejemplo 4.4 - Sistema de Sistemas </summary>

#### Ejemplo 4.4 - a System of Systems

---

<details>
  <summary>🔍 Gestión de memoria</summary>

Este ejemplo introduce un concepto más complejo: **un sistema de sistemas**, o un array de emisores donde cada emisor tiene su propio array de partículas.

**1. Creación de emisores:**
```javascript
function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}
```
- Cada clic del mouse crea un **nuevo emisor** en esa posición
- Los emisores se agregan al array `emitters[]`
- Cada emisor es independiente y tiene su propia posición de origen

**2. Creación de partículas dentro de cada emisor:**
```javascript
emitter.addParticle();  // Llamado para cada emisor en draw()
```
- Cada emisor agrega partículas a su **propio** array interno `this.particles[]`
- Las partículas nacen en la posición `origin` del emisor

**3. Desaparición de partículas:**
```javascript
for (let i = this.particles.length - 1; i >= 0; i--) {
  let p = this.particles[i];
  p.run();
  if (p.isDead()) {
    this.particles.splice(i, 1);  // Elimina partícula individual
  }
}
```
- Cada emisor gestiona la eliminación de **sus propias partículas muertas**
- Se usa `splice()` al recorrer el array hacia atrás

**4. Observación:**
- **Los emisores NUNCA se eliminan**: El array `emitters[]` crece indefinidamente  
- Si haces 100 clics, tendrás 100 emisores activos
- Aunque sus partículas mueran, el emisor sigue existiendo
- Solución ideal: eliminar emisores cuando ya no tengan partículas vivas

**Diagrama de estructura:**
```
emitters[] (Array de Emisores)
   ↓
   ├─ Emitter 1
   │    └─ particles[] → [Particle, Particle, Particle...]
   │
   ├─ Emitter 2
   │    └─ particles[] → [Particle, Particle...]
   │
   └─ Emitter 3
        └─ particles[] → [Particle, Particle, Particle, Particle...]
```

**Gestión de memoria en dos niveles:**
1. **Nivel superior**: Emisores en el array global `emitters[]`
2. **Nivel inferior**: Partículas en el array interno de cada emisor `this.particles[]`

</details>

---

<details>
  <summary>🧪 Concepto aplicado: Normalización de Vectores (Unidad 2)</summary>

**Experimento: Control de dirección con vectores normalizados**

**¿Por qué elegí este concepto?**

En el ejemplo original, las partículas tienen velocidades aleatorias sin control direccional:
```javascript
this.velocity = createVector(random(-2, 2), random(-3, 0));
```

Quería que las partículas explotaran en **todas las direcciones radialmente** desde el emisor, como erupcion de volcanes reales, manteniendo velocidades consistentes.

**¿Cómo lo implementé?**

**Paso 1: Generar un vector desde un ángulo aleatorio**
```javascript
let angle = random(TWO_PI);  // Ángulo aleatorio de 0 a 360°
this.velocity = p5.Vector.fromAngle(angle);
```
- `fromAngle()` crea un vector unitario (magnitud = 1) apuntando en el ángulo especificado
- Esto garantiza que todas las partículas apunten en direcciones diferentes

**Paso 2: Normalizar para garantizar magnitud = 1**
```javascript
this.velocity.normalize();  // Asegura magnitud exactamente 1
```
- Aunque `fromAngle()` ya crea vectores normalizados, esta línea es una buena práctica
- La normalización convierte cualquier vector a magnitud 1 manteniendo su dirección

**Paso 3: Escalar a la velocidad deseada**
```javascript
this.velocity.mult(random(1, 4));  // Velocidad entre 1 y 4
```
- Multiplica el vector normalizado por un escalar
- Esto mantiene la dirección pero ajusta la rapidez

**Concepto matemático:**

Un vector normalizado tiene **magnitud = 1**:
```
v = (x, y)
magnitud = √(x² + y²) = 1

Normalización: v̂ = v / |v|
```

Esto es útil porque:
- ✅ Separa **dirección** (el vector normalizado) de **velocidad** (el escalar)
- ✅ Permite control independiente de ambos aspectos
- ✅ Evita velocidades inconsistentes por valores aleatorios extremos

**Comparación visual:**

| Sin normalización | Con normalización |
|-------------------|-------------------|
| Velocidades aleatorias en X e Y | Explosión radial uniforme |
| Algunas partículas más rápidas | Control preciso de velocidad |
| Dirección sesgada | Distribución equitativa en 360° |

**Comparación con random():**
- `random()`: Crea vectores con componentes X e Y independientes, sin control de dirección
- `fromAngle() + normalize()`: Control total sobre dirección y velocidad por separado

</details>

---

<details>
  <summary>💻 Código completo</summary>

```javascript
let emitters = [];

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);
  
  // Recorrer todos los emisores
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticle();
  }
  
  // Mostrar info
  fill(0);
  noStroke();
  text(`Emisores: ${emitters.length}`, 10, 20);
  text(`Click para crear emisor`, 10, 40);
}

function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}

// ========================================
// CLASE EMITTER
// ========================================
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }
  
  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }
  
  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// ========================================
// CLASE PARTICLE CON VECTORES NORMALIZADOS
// ========================================
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    
    // 🆕 NOVEDAD: Usar normalización para control direccional
    let angle = random(TWO_PI);  // Ángulo aleatorio en 360°
    this.velocity = p5.Vector.fromAngle(angle);  // Vector unitario
    this.velocity.normalize();  // Asegurar magnitud = 1
    this.velocity.mult(random(1, 4));  // Escalar a velocidad deseada
    
    this.acceleration = createVector(0, 0.05);  // Gravedad
    this.lifespan = 255;
  }
  
  run() {
    this.update();
    this.show();
  }
  
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
  }
  
  show() {
    stroke(255, 100, 150, this.lifespan);
    strokeWeight(2);
    fill(255, 150, 200, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }
  
  isDead() {
    return this.lifespan < 0;
  }
}
```

**🔗 Enlace p5.js:** [Ver código en vivo](https://editor.p5js.org/DanieLudens/sketches/iKfXE60bT) 

</details>

---

📸 **Resultados visuales**

**visuales:**
- ✅ **Simetría radial**: Las partículas se distribuyen uniformemente en todas las direcciones
- ✅ **Control de velocidad**: Todas las partículas tienen velocidades dentro del rango especificado (1-4)
- ✅ **Efecto de fuegos artificiales**: Cada clic crea una "explosión" visualmente satisfactoria
- ✅ **Colores rosados**: Paleta diferente para distinguir este ejemplo del anterior

**Diferencias clave:**
- **Sin normalización:** Las velocidades aleatorias pueden crear sesgos direccionales
- **Con normalización:** Explosión perfectamente simétrica desde el punto de origen

</details>

|Original|Modificado|
|-----|-----|
|<img width="400" src="https://github.com/user-attachments/assets/21a05e24-64f3-4809-81ec-ee9b94782b50">|<img width="400" src="https://github.com/user-attachments/assets/3e6148c1-0ae3-41c1-ab2f-4c9097134a4b">|





<details>
  <summary>Ejemplo 4.5 - Herencia y Polimorfismo </summary>

#### Ejemplo 4.5 - a Particle System with Inheritance and Polymorphism

---

<details>
  <summary>🔍 Gestión de memoria</summary>

Este ejemplo mantiene la misma gestión de memoria que ejemplos anteriores, pero introduce una diferencia importante: **un solo array contiene DOS tipos diferentes de objetos**.

**1. Creación de partículas de diferentes tipos:**
```javascript
addParticle() {
  let r = random(1);
  if (r < 0.5) {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  } else {
    this.particles.push(new Confetti(this.origin.x, this.origin.y));
  }
}
```
- 50% de probabilidad de crear una `Particle` (círculo)
- 50% de probabilidad de crear un `Confetti` (cuadrado rotado)
- Ambos tipos se agregan al **mismo array** `particles[]`

**2. Polimorfismo en el array:**
```javascript
this.particles = [Particle, Confetti, Particle, Confetti, Particle, ...];
```
- JavaScript permite mezclar tipos de objetos en un array
- Gracias a la herencia, ambos son tratados como tipo `Particle`
- Cada objeto mantiene su comportamiento único (círculos vs cuadrados)

**3. Desaparición - Sin cambios:**
```javascript
if (particle.isDead()) {
  this.particles.splice(i, 1);
}
```
- El método `isDead()` es **heredado** de la clase `Particle`
- Funciona igual para `Particle` y `Confetti`
- No importa el tipo, todos se eliminan de la misma manera

**4. Observación sobre herencia:**
La clase `Confetti` NO necesita su propio `isDead()` ni `update()`:
- ✅ **Hereda** `isDead()` de `Particle`
- ✅ **Hereda** `update()` de `Particle`
- ✅ Solo **sobrescribe** `show()` para cambiar la apariencia

**Diagrama de herencia:**
```
Particle (Clase Padre)
   ├─ position, velocity, acceleration, lifespan
   ├─ update()
   ├─ isDead()
   └─ show() → dibuja círculo
        ↓
   Confetti (Clase Hija) extends Particle
   ├─ HEREDA: position, velocity, acceleration, lifespan
   ├─ HEREDA: update(), isDead()
   └─ SOBRESCRIBE: show() → dibuja cuadrado rotado
```

</details>

---

<details>
  <summary>🧪 Concepto aplicado: Fricción/Resistencia del Aire (Unidad 3)</summary>

**Experimento: Fricción para desacelerar partículas**

**¿Por qué elegí este concepto?**

En el ejemplo original, las partículas mantienen su velocidad constante (solo afectadas por gravedad). Quería agregar **resistencia del aire** para que las partículas se desaceleren gradualmente, simulando un comportamiento más realista.

**¿Cómo lo implementé?**

La fricción es una fuerza que se opone al movimiento. Su magnitud es proporcional a la velocidad del objeto:

**Fórmula de fricción:**
```
fricción = -μ × velocidad × |velocidad|
```
Donde:
- `μ` (mu) = coeficiente de fricción
- La fricción apunta en dirección **opuesta** a la velocidad
- Es proporcional al **cuadrado** de la velocidad

**Implementación en código:**

**Paso 1: Crear el método applyFriction() en Particle**
```javascript
applyFriction() {
  let friction = this.velocity.copy();  // Copiar vector velocidad
  friction.normalize();                  // Obtener dirección (magnitud = 1)
  friction.mult(-1);                     // Invertir (opuesta al movimiento)
  
  let c = 0.01;  // Coeficiente de fricción
  let speed = this.velocity.mag();       // Obtener velocidad actual
  friction.mult(c * speed);              // Fricción proporcional a velocidad
  
  this.acceleration.add(friction);       // Aplicar como fuerza
}
```

**Explicación detallada del código:**
1. `velocity.copy()` - Copia el vector para no modificar el original
2. `normalize()` - Convierte a vector unitario (magnitud = 1) para obtener solo la dirección
3. `mult(-1)` - Invierte la dirección (fricción opuesta al movimiento)
4. `velocity.mag()` - Obtiene la magnitud/velocidad actual
5. `mult(c * speed)` - Escala la fricción proporcionalmente a la velocidad
6. `acceleration.add()` - Suma la fricción a la aceleración total

**Paso 2: Llamar applyFriction() en update()**
```javascript
update() {
  this.applyFriction();  // 1. Aplicar fricción primero
  this.velocity.add(this.acceleration);  // 2. Actualizar velocidad
  this.position.add(this.velocity);      // 3. Actualizar posición
  this.acceleration.mult(0);              // 4. Resetear aceleración
  this.lifespan -= 2;                     // 5. Reducir tiempo de vida
}
```

**Comparación visual:**

| Sin fricción | Con fricción (c = 0.01) |
|--------------|-------------------------|
| Partículas mantienen velocidad | Partículas se desaceleran gradualmente |
| Trayectorias largas y rectas | Trayectorias que se "frenan" |
| Movimiento constante | Movimiento más natural y realista |

**Nota técnica:**

La implementación actual usa fricción proporcional a la velocidad (`F = c × v`). En física real, la resistencia del aire es proporcional a `v²`. Aquí está la comparación:

```javascript
// Implementación actual (más simple)
friction.mult(c * speed);  // F ∝ v

// Implementación más realista (opcional)
friction.mult(c * speed * speed);  // F ∝ v²
```

La versión con `v²` produce una desaceleración más dramática a altas velocidades, pero puede ser visualmente menos sutil.

</details>

---

<details>
  <summary>💻 Código completo</summary>

```javascript
let emitter;

function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 50);
}

function draw() {
  background(255);
  emitter.addParticle();
  emitter.run();
  
  // Info
  fill(0);
  noStroke();
  text(`Partículas: ${emitter.particles.length}`, 10, 20);
}

// ========================================
// CLASE EMITTER
// ========================================
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }
  
  addParticle() {
    let r = random(1);
    if (r < 0.5) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    }
  }
  
  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let particle = this.particles[i];
      particle.run();
      
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// ========================================
// CLASE PARTICLE CON FRICCIÓN
// ========================================
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(4, 0));  // 🆕 Velocidad inicial alta para ver fricción
    this.acceleration = createVector(0, 0.05);
    this.lifespan = 255;
  }
  
  run() {
    this.update();
    this.show();
  }
  
  // 🆕 MÉTODO DE FRICCIÓN
  applyFriction() {
    let friction = this.velocity.copy();  // Copiar el vector velocidad
    friction.normalize();                  // Convertir a vector unitario
    friction.mult(-1);                     // Invertir dirección (opuesta al movimiento)
    
    let c = 0.01;  // Coeficiente de fricción
    let speed = this.velocity.mag();       // Obtener magnitud de velocidad
    friction.mult(c * speed);              // Fricción proporcional a velocidad
    
    this.acceleration.add(friction);       // Aplicar fricción como fuerza
  }
  
  update() {
    this.applyFriction();  // 🆕 Aplicar fricción primero
    this.velocity.add(this.acceleration);  // Actualizar velocidad con aceleración
    this.position.add(this.velocity);      // Actualizar posición con velocidad
    this.acceleration.mult(0);              // Resetear aceleración para el próximo frame
    this.lifespan -= 2;                     // Reducir tiempo de vida
  }
  
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 12);
  }
  
  isDead() {
    return this.lifespan < 0;
  }
}

// ========================================
// CLASE CONFETTI (HEREDA FRICCIÓN)
// ========================================
class Confetti extends Particle {
  constructor(x, y) {
    super(x, y);
    // Confetti automáticamente hereda applyFriction()
  }
  
  // Sobrescribe solo show() para cambiar apariencia
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);
    
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(175, 0, 175, this.lifespan);  // Color morado para distinguir
    
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    rectMode(CENTER);
    square(0, 0, 12);
    pop();
  }
}
```

**🔗 Enlace p5.js:** [Ver en vivo](https://editor.p5js.org/DanieLudens/sketches/Mvsl_dgqN)

</details>

---

📸 **Resultados visuales**

**visuales:**
- ✅ **Dos tipos de partículas**: Círculos grises y cuadrados morados rotados
- ✅ **Desaceleración muy visible**: Las partículas empiezan cayendo rápido y se frenan notoriamente
- ✅ **Efecto realista**: Simula resistencia del aire de manera convincente
- ✅ **Velocidad inicial alta**: Con `random(4, 0)` el efecto de fricción es dramático

**Diferencias clave:**
- **Sin fricción:** Partículas mantienen velocidad constante, caen en líneas casi rectas
- **Con fricción + velocidad alta:** Partículas se desaceleran progresivamente, creando trayectorias parabólicas naturales

</details>


|Original|Modificado|
|-----|-----|
|<img width="400" src="https://github.com/user-attachments/assets/89bd2838-e372-46b7-961d-88fe33b9aaac">|<img width="400" src="https://github.com/user-attachments/assets/0b959b23-1765-4153-a4db-4e91b777f0ac">|






<details>
  <summary>Ejemplo 4.6 - Sistema con Fuerzas </summary>

#### Ejemplo 4.6 - a Particle System with Forces

---

<details>
  <summary>🔍 Gestión de memoria</summary>

Este ejemplo mantiene la misma gestión de memoria que ejemplos anteriores, pero introduce una **arquitectura diferente para aplicar fuerzas**. Sin embargo la getion de fuerzas no afecta la gestion de memoria en este ejemplo.

**1. Estructura de fuerzas:**
```javascript
function draw() {
  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);  // Fuerza aplicada DESDE draw()
}
```
- Las fuerzas se crean en `draw()` (nivel más alto)
- Se envían al `Emitter` mediante `applyForce()`
- El `Emitter` las distribuye a todas las partículas

**2. Flujo de la fuerza (cascada):**
```
draw()
  ↓ crea fuerza
emitter.applyForce(force)
  ↓ recibe y distribuye
  for (cada partícula) {
    particle.applyForce(force)
  }
```

**3. Método applyForce() en Emitter:**
```javascript
applyForce(force) {
  for (let particle of this.particles) {
    particle.applyForce(force);  // Aplica a cada una
  }
}
```
- Itera sobre TODAS las partículas
- Aplica la misma fuerza a cada una
- Usa `for...of` porque no elimina elementos

**4. Método applyForce() en Particle:**
```javascript
applyForce(force) {
  let f = force.copy();    // Copiar para no modificar el original
  f.div(this.mass);        // a = F/m
  this.acceleration.add(f);
}
```
- Copia la fuerza (importante: no modificar el vector original)
- Divide por masa (Segunda Ley de Newton: F = ma)
- Suma a la aceleración acumulada

**5. Observación importante - Masa:**
En este ejemplo se agrega la propiedad `mass`:
```javascript
this.mass = 1;
```
- Permite que diferentes partículas reaccionen diferente a la misma fuerza
- Si `mass = 2`, la aceleración es la mitad
- Si `mass = 0.5`, la aceleración es el doble

**6. Gestión de memoria - Sin cambios:**
```javascript
if (particle.isDead()) {
  this.particles.splice(i, 1);
}
```
- Las partículas se eliminan igual que antes
- La gestión de fuerzas NO afecta la gestión de memoria

**Diagrama de arquitectura:**
```
┌─────────────┐
│   draw()    │ Crea fuerzas (gravedad, viento, etc.)
└──────┬──────┘
       ↓ emitter.applyForce(force)
┌─────────────┐
│   Emitter   │ Distribuye fuerza a todas las partículas
└──────┬──────┘
       ↓ particle.applyForce(force) × N veces
┌─────────────┐
│  Particle   │ Aplica F/m a su aceleración
└─────────────┘
```

</details>

---

<details>
  <summary>🧪 Concepto aplicado: Movimiento Armónico Simple / Oscilador (Unidad 4)</summary>

**Experimento: Emisor oscilante con funciones trigonométricas**

**¿Por qué elegí este concepto?**

En el ejemplo original, el emisor está **estático** en una posición fija. Quería que el emisor se moviera de lado a lado creando un **movimiento ondulatorio**, como un péndulo o una onda, para que las partículas se emitieran desde posiciones variables creando patrones visuales más interesantes.

**¿Cómo lo implementé?**

El movimiento armónico simple se basa en funciones trigonométricas (seno y coseno) que oscilan entre -1 y 1:

**Fórmula del oscilador:**
```
x(t) = amplitud × sin(ángulo)
ángulo = velocidadAngular × tiempo
```

**Implementación paso a paso:**

**Paso 1: Agregar propiedades al Emitter**
```javascript
constructor(x, y) {
  this.origin = createVector(x, y);
  this.particles = [];
  
  // 🆕 Propiedades para oscilación
  this.angle = 0;              // Ángulo actual
  this.angleVel = 0.05;        // Velocidad angular
  this.amplitude = 100;        // Amplitud de oscilación
}
```

**Paso 2: Actualizar posición con seno**
```javascript
update() {
  // Calcular nueva posición X usando seno
  let x = this.origin.x + sin(this.angle) * this.amplitude;
  
  // Actualizar posición actual del emisor
  this.position = createVector(x, this.origin.y);
  
  // Incrementar ángulo (movimiento continuo)
  this.angle += this.angleVel;
}
```

**Paso 3: Emitir partículas desde la nueva posición**
```javascript
addParticle() {
  // Usar this.position en lugar de this.origin
  this.particles.push(new Particle(this.position.x, this.position.y));
}
```

**Conceptos matemáticos:**

**Función seno:**
```
sin(0) = 0
sin(π/2) = 1
sin(π) = 0
sin(3π/2) = -1
sin(2π) = 0  (ciclo completo)
```

**Movimiento resultante:**
```
Tiempo: 0  → x = centro + sin(0) × 100 = centro
Tiempo: 1  → x = centro + sin(0.05) × 100 = centro + 5
Tiempo: 2  → x = centro + sin(0.10) × 100 = centro + 10
...
```

**Parámetros que probé:**

| Parámetro | Valor | Efecto visual |
|-----------|-------|---------------|
| `amplitude = 50` | Bajo | Oscilación sutil, movimiento contenido |
| `amplitude = 100` | Moderado | Oscilación visible y equilibrada ✅ |
| `amplitude = 200` | Alto | Oscilación muy amplia, casi llega a los bordes |
| `angleVel = 0.02` | Lento | Movimiento suave y lento |
| `angleVel = 0.05` | Moderado | Velocidad natural y visible ✅ |
| `angleVel = 0.1` | Rápido | Oscilación muy rápida, casi frenética |

**¿Por qué funciona?**

1. **`sin(angle)`** oscila naturalmente entre -1 y 1
2. **Multiplicar por amplitud** escala el rango: `[-amplitude, +amplitude]`
3. **Sumar a origin.x** centra la oscilación: `[origin.x - amplitude, origin.x + amplitude]`
4. **Incrementar angle** crea movimiento continuo

**Comparación visual:**

| Sin oscilación | Con oscilación (amplitude = 100) |
|----------------|----------------------------------|
| Emisor estático en el centro | Emisor se mueve de lado a lado |
| Partículas caen en línea recta | Partículas crean patrón ondulado |
| Monótono | Dinámico y visualmente interesante |

**Variaciones que experimenté:**

**1. Oscilación vertical:**
```javascript
let y = this.origin.y + sin(this.angle) * this.amplitude;
this.position = createVector(this.origin.x, y);
```
Resultado: Emisor sube y baja

**2. Oscilación en ambos ejes (Lissajous):**
```javascript
let x = this.origin.x + sin(this.angle) * this.amplitude;
let y = this.origin.y + cos(this.angle * 1.5) * this.amplitude;
this.position = createVector(x, y);
```
Resultado: Patrón de figura 8 o elipse

**3. Amplitud variable:**
```javascript
this.amplitude = 50 + sin(this.angle * 0.5) * 50;
```
Resultado: La amplitud de oscilación también oscila (meta-oscilación)

**Resultado final:**

Las partículas ahora se emiten desde un emisor que se mueve sinusoidalmente, creando un **patrón de cortina ondulante** o "fuente danzante". El movimiento armónico simple genera un efecto agradable a la vista.

**Concepto físico:**

Este es el mismo principio que gobierna:
- Péndulos
- Muelles/resortes
- Ondas de agua
- Movimiento circular proyectado en un eje

</details>

---

<details>
  <summary>💻 Código completo</summary>

```javascript
let emitter;

function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 50);
}

function draw() {
  background(255);
  
  // Aplicar gravedad a todas las partículas
  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);
  
  // 🆕 Actualizar posición del emisor (oscilación)
  emitter.update();
  
  emitter.addParticle();
  emitter.run();
  emitter.show();  // 🆕 Mostrar el emisor
  
  // Info
  fill(0);
  noStroke();
  text(`Partículas: ${emitter.particles.length}`, 10, 20);
}

// ========================================
// CLASE EMITTER CON OSCILACIÓN
// ========================================
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);  // Posición central
    this.position = createVector(x, y); // Posición actual
    this.particles = [];
    
    // 🆕 Propiedades para oscilación
    this.angle = 0;              // Ángulo actual
    this.angleVel = 0.05;        // Velocidad angular
    this.amplitude = 100;        // Amplitud de oscilación
  }
  
  // 🆕 Actualizar posición con movimiento armónico
  update() {
    // Calcular nueva posición X usando seno
    let x = this.origin.x + sin(this.angle) * this.amplitude;
    this.position.x = x;
    
    // Incrementar ángulo para movimiento continuo
    this.angle += this.angleVel;
  }
  
  addParticle() {
    // Emitir desde la posición actual (oscilante)
    this.particles.push(new Particle(this.position.x, this.position.y));
  }
  
  applyForce(force) {
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }
  
  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let particle = this.particles[i];
      particle.run();
      
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
  
  // 🆕 Mostrar el emisor
  show() {
    push();
    stroke(0);
    strokeWeight(2);
    fill(200, 0, 200, 150);
    circle(this.position.x, this.position.y, 20);
    pop();
  }
}

// ========================================
// CLASE PARTICLE CON MASA
// ========================================
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-2, 0));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255;
    this.mass = 1;  // Masa para F = ma
  }
  
  run() {
    this.update();
    this.show();
  }
  
  applyForce(force) {
    let f = force.copy();    // Copiar para no modificar original
    f.div(this.mass);        // a = F/m
    this.acceleration.add(f);
  }
  
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);  // Resetear aceleración
    this.lifespan -= 2;
  }
  
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(100, 150, 255, this.lifespan);
    circle(this.position.x, this.position.y, 12);
  }
  
  isDead() {
    return this.lifespan < 0;
  }
}
```

**🔗 Enlace p5.js:** [Ver en vivo](https://editor.p5js.org/DanieLudens/sketches/XLn26vldA)

</details>

---

📸 **Resultados visuales**

**visuales:**
- ✅ **Emisor visible**: Círculo morado oscilante que muestra la fuente de partículas
- ✅ **Patrón ondulado**: Las partículas crean una "cortina" con forma de onda
- ✅ **Movimiento hipnótico**: El oscilador crea un efecto naturalmente agradable
- ✅ **Combinación de fuerzas**: Oscilación horizontal + gravedad vertical = trayectorias parabólicas
- ✅ **Efecto de fuente danzante**: Similar a fuentes ornamentales con movimiento

**Diferencias clave:**
- **Sin oscilación:** Partículas caen desde un punto fijo, patrón predecible
- **Con oscilación:** Partículas caen desde posiciones variables, creando patrón ondulante dinámico

**movimiento:**
El emisor se mueve siguiendo la ecuación `x = centro + sin(t) × amplitud`, creando un movimiento armónico simple. Las partículas heredan la posición X del emisor en el momento de su creación, y luego caen por gravedad, resultando en un patrón visual que parece una onda congelada en el tiempo.

</details>

|Original|Modificado|
|-----|-----|
|<img width="400" src="https://github.com/user-attachments/assets/cff0c19e-f9d3-46f0-b2a2-8319d2f23668">|<img width="400" src="https://github.com/user-attachments/assets/b8a5814c-9e7e-41b2-8c8b-1c56f1dcd068">|


<details>
  <summary>Ejemplo 4.7 - Sistema con Repeledor</summary>

#### Ejemplo 4.7 - a Particle System with a Repeller

---

<details>
  <summary>🔍 Gestión de memoria</summary>

Este ejemplo mantiene la misma gestión de memoria pero introduce un nuevo tipo de objeto: el **Repeller** (repeledor).

**1. Estructura con Repeller:**
```javascript
let emitter;   // Sistema de partículas
let repeller;  // Objeto que repele partículas
```
- El `Repeller` es un objeto **independiente** del sistema de partículas
- No se almacena en ningún array (es único)
- Se crea una sola vez en `setup()` y persiste

**2. Aplicación de fuerzas en dos niveles:**

**Nivel 1 - Gravedad (uniforme):**
```javascript
let gravity = createVector(0, 0.1);
emitter.applyForce(gravity);  // Misma fuerza para todas
```

**Nivel 2 - Repulsión (específica):**
```javascript
emitter.applyRepeller(repeller);  // Fuerza diferente por partícula
```

**3. Método applyRepeller() en Emitter:**
```javascript
applyRepeller(repeller) {
  for (let particle of this.particles) {
    let force = repeller.repel(particle);  // Calcular fuerza única
    particle.applyForce(force);            // Aplicar a esta partícula
  }
}
```
- Itera sobre cada partícula
- Calcula una fuerza **específica** para cada una según su distancia al repeller
- Aplica la fuerza individualmente

**4. Diferencia con applyForce():**

| `applyForce(force)` | `applyRepeller(repeller)` |
|---------------------|---------------------------|
| Recibe un vector | Recibe un objeto Repeller |
| Aplica la misma fuerza a todas | Calcula fuerza diferente para cada una |
| `force` es constante | `force` depende de la distancia |

**5. Gestión de memoria - Sin cambios:**
```javascript
if (particle.isDead()) {
  this.particles.splice(i, 1);
}
```
- Las partículas se eliminan igual que en ejemplos anteriores
- El `Repeller` NO se elimina (objeto permanente)

**6. Diagrama de arquitectura:**
```
┌─────────────┐
│   draw()    │ 
└──────┬──────┘
       ├→ Gravedad uniforme → emitter.applyForce(gravity)
       └→ Repulsión variable → emitter.applyRepeller(repeller)
                                       ↓
                               for (cada partícula) {
                                 force = repeller.repel(partícula)
                                 partícula.applyForce(force)
                               }
```

**Observación importante:**
El `Repeller` NO gestiona partículas, solo **calcula fuerzas** cuando se le pide. Es el `Emitter` quien se encarga de pedirle al repeller que calcule la fuerza para cada partícula y luego aplicarla.

</details>

---

<details>
  <summary>🧪 Concepto aplicado: Repeller Oscilante (Unidad 4)</summary>

**Experimento: Repeller con movimiento armónico simple horizontal**

**¿Por qué elegí este concepto?**

En el ejemplo original, el repeller está **estático** en una posición fija. Quería que el repeller se moviera horizontalmente con un **movimiento oscilatorio**, creando un "escudo móvil" que empuja las partículas de manera dinámica, generando patrones visuales más complejos e interesantes.

**¿Cómo lo implementé?**

El repeller usa la misma técnica de movimiento armónico simple que usamos en el Ejemplo 4.6, pero aplicada a un objeto que repele en lugar de emitir.

**Implementación paso a paso:**

**Paso 1: Agregar propiedades al Repeller**
```javascript
constructor(x, y) {
  this.origin = createVector(x, y);    // Posición central fija
  this.position = createVector(x, y);  // Posición actual (oscilante)
  this.power = 150;                    // Fuerza de repulsión
  
  // 🆕 Propiedades para oscilación
  this.angle = 0;          // Ángulo actual
  this.angleVel = 0.03;    // Velocidad angular (más lento que el emisor)
  this.amplitude = 80;     // Amplitud de oscilación
}
```

**Paso 2: Método update() para movimiento**
```javascript
update() {
  // Calcular nueva posición X usando seno
  let x = this.origin.x + sin(this.angle) * this.amplitude;
  this.position.x = x;
  
  // Incrementar ángulo
  this.angle += this.angleVel;
}
```

**Paso 3: Llamar update() en draw()**
```javascript
function draw() {
  // ...
  repeller.update();  // 🆕 Actualizar posición del repeller
  emitter.applyRepeller(repeller);
  // ...
}
```

**Fórmula de repulsión:**

La fuerza de repulsión sigue la ley de gravitación inversa al cuadrado:
```
F = -G / d²

Donde:
- G = power (150)
- d = distancia entre repeller y partícula
- El signo negativo hace que REPELE (empuja hacia afuera)
```

**Cálculo paso a paso en repel():**
```javascript
// 1. Vector del repeller a la partícula
let force = p5.Vector.sub(this.position, particle.position);
// Si partícula está a la derecha → force apunta a la derecha
// Si partícula está a la izquierda → force apunta a la izquierda

// 2. Distancia
let distance = force.mag();
distance = constrain(distance, 5, 50);  // Evitar fuerzas infinitas

// 3. Magnitud de la fuerza (negativa = repele)
let strength = -1 * this.power / (distance * distance);

// 4. Aplicar magnitud al vector de dirección
force.setMag(strength);
```

**Parámetros que probé:**

| Parámetro | Valor | Efecto visual |
|-----------|-------|---------------|
| `power = 100` | Bajo | Repulsión débil, partículas pasan cerca |
| `power = 150` | Moderado | Repulsión equilibrada ✅ |
| `power = 300` | Alto | Repulsión fuerte, partículas desviadas bruscamente |
| `angleVel = 0.02` | Muy lento | Movimiento casi imperceptible |
| `angleVel = 0.03` | Lento | Movimiento suave y visible ✅ |
| `angleVel = 0.05` | Moderado | Movimiento más rápido |
| `amplitude = 50` | Pequeña | Oscilación sutil |
| `amplitude = 80` | Moderada | Oscilación visible ✅ |
| `amplitude = 150` | Grande | Oscilación muy amplia |

**Resultado final:**

Las partículas caen por gravedad pero son **desviadas** por el repeller oscilante. El efecto es como si hubiera un "escudo invisible" moviéndose de lado a lado, donde las partículas son empujadas hacia los lados cuando el repeller pasa cerca.

**Concepto físico similar:**
- Carga eléctrica negativa repeliendo electrones
- Campo magnético repeliendo imanes del mismo polo
- Escudo de fuerza en ciencia ficción

</details>

---

<details>
  <summary>💻 Código completo</summary>

```javascript
let emitter;
let repeller;

function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 20);
  repeller = new Repeller(width / 2, 150);
}

function draw() {
  background(255);
  
  emitter.addParticle();
  
  // Aplicar gravedad (fuerza uniforme)
  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);
  
  // 🆕 Actualizar posición del repeller (oscilación)
  repeller.update();
  
  // Aplicar repulsión (fuerza específica por partícula)
  emitter.applyRepeller(repeller);
  
  emitter.run();
  repeller.show();
  
  // Info
  fill(0);
  noStroke();
  text(`Partículas: ${emitter.particles.length}`, 10, 20);
}

// ========================================
// CLASE EMITTER
// ========================================
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }
  
  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }
  
  applyForce(force) {
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }
  
  // Aplicar repulsión desde el repeller
  applyRepeller(repeller) {
    for (let particle of this.particles) {
      let force = repeller.repel(particle);
      particle.applyForce(force);
    }
  }
  
  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let particle = this.particles[i];
      particle.run();
      
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// ========================================
// CLASE REPELLER CON OSCILACIÓN
// ========================================
class Repeller {
  constructor(x, y) {
    this.origin = createVector(x, y);    // Posición central fija
    this.position = createVector(x, y);  // Posición actual (oscilante)
    this.power = 150;                    // Fuerza de repulsión
    
    // 🆕 Propiedades para oscilación
    this.angle = 0;          // Ángulo actual
    this.angleVel = 0.03;    // Velocidad angular (más lento que emisor)
    this.amplitude = 80;     // Amplitud de oscilación
  }
  
  // 🆕 Actualizar posición con movimiento armónico simple
  update() {
    // Calcular nueva posición X usando seno
    let x = this.origin.x + sin(this.angle) * this.amplitude;
    this.position.x = x;
    
    // Incrementar ángulo para movimiento continuo
    this.angle += this.angleVel;
  }
  
  show() {
    push();
    stroke(0);
    strokeWeight(2);
    fill(255, 0, 0, 100);  // Rojo semi-transparente
    circle(this.position.x, this.position.y, 32);
    
    // Línea para mostrar el rango de oscilación
    stroke(255, 0, 0, 50);
    line(this.origin.x - this.amplitude, this.position.y,
         this.origin.x + this.amplitude, this.position.y);
    pop();
  }
  
  // Calcular fuerza de repulsión para UNA partícula específica
  repel(particle) {
    // 1. Vector de repeller hacia partícula
    let force = p5.Vector.sub(this.position, particle.position);
    
    // 2. Obtener y limitar distancia
    let distance = force.mag();
    distance = constrain(distance, 5, 50);
    
    // 3. Calcular magnitud (inverso al cuadrado de la distancia)
    // Negativo = repele (empuja hacia afuera)
    let strength = -1 * this.power / (distance * distance);
    
    // 4. Establecer magnitud al vector de fuerza
    force.setMag(strength);
    
    return force;
  }
}

// ========================================
// CLASE PARTICLE
// ========================================
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-2, 0));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255;
    this.mass = 1;
  }
  
  run() {
    this.update();
    this.show();
  }
  
  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }
  
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 2;
  }
  
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(100, 200, 100, this.lifespan);  // Verde
    circle(this.position.x, this.position.y, 12);
  }
  
  isDead() {
    return this.lifespan < 0;
  }
}
```

**🔗 Enlace p5.js:** [Ver en vivo](https://editor.p5js.org/DanieLudens/sketches/U9ZVqJ2IT)

</details>

---

📸 **Resultados visuales**

**visuales:**
- ✅ **Repeller visible**: Círculo rojo oscilante que repele partículas
- ✅ **Línea de referencia**: Muestra el rango de oscilación del repeller
- ✅ **Patrones de flujo**: Las partículas son desviadas creando formas ondulantes
- ✅ **Efecto de escudo móvil**: Como un campo de fuerza que se mueve horizontalmente
- ✅ **Interacción dinámica**: Gravedad + repulsión = trayectorias complejas

**Diferencias clave:**
- **Repeller estático:** Partículas desviadas en patrón de V fijo
- **Repeller oscilante:** Patrones de flujo cambiantes, las partículas son empujadas en diferentes direcciones según la posición del repeller

</details>

|Original|Modificado|
|-----|-----|
|<img width="400" src="https://github.com/user-attachments/assets/0e2b0464-86a7-42a9-925b-73779f208c91">|<img width="400" src="https://github.com/user-attachments/assets/2432db44-7275-4900-a8c3-5dbed59cd56a">|





--- 

## Apply: Aplicación 🛠

### Actividad 03

Es hora de una nueva creación. Diseña e implementa una obra de arte generativa algorítmica interactiva en tiempo real en p5.js que cumpla con los siguientes requisitos:

Documenta el proceso de creación, incluyendo la idea inicial, bocetos, experimentación con el código y el resultado final.

1. Es unidad incluye una novedad: DISEÑO. Debes intencionar tu obra. Esta vez te pediré que DISEÑES antes de generar código. Define un concepto, haz bocetos, define la interacción, etc. ¿Cuál es el concepto de tu obra? ¿Qué quieres comunicar con ella?

2. Debes utilizar los conceptos de herencia y polimorfismo que revisaste en la fase de investigación.

3. Debes utilizar al menos un concepto de cada una de las unidades anteriores: 4 conceptos.

4. Debes definir cómo vas a gestionar el tiempo de vida de las partículas y la memoria.

5. La obra debe ser interactiva en tiempo real. Puedes usar teclado, mouse, música, el micrófono, video, sensor o cualquier otro dispositivo de entrada. 

6. Incluye un enlace a tu código en el editor de p5.js.

7. Incluye el código fuente.

8. Captura de pantallas de tu obra con las imágenes que más te gusten



## Corrientes de flujo musical - Arte generativo reactivo al audio

<img width="1000" src="https://github.com/user-attachments/assets/a3fcc130-912a-4a0d-bff4-69d1e2d01d76">

### 📐 FASE 1: DISEÑO

<details>
<summary><strong>🎨 Concepto de la Obra</strong></summary>

**Concepto:** Corrientes de flujo musical

**¿Qué quiero comunicar?**

Una visualización del sonido ó la música donde no solo se escucha, sino que se ve como va tomando rumbo y forma.. Cada frecuencia de audio (graves, medios, agudos) tiene su propia "voz visual" en forma de partículas que fluyen, danzan y reaccionan al ritmo de la música Donde el sonido se convierte en movimiento y color.

**Inspiración:**

- El concepto de "ver la música" - sinestesia
- Flow fields naturales como corrientes de agua o viento
- El trabajo de artistas como [Refik Anadol](https://refikanadol.com/) en arte generativo reactivo
- [Waves - Jerome](https://openprocessing.org/sketch/2442420) - Open Processing

<img width="300" src="https://github.com/user-attachments/assets/42d0f356-62dc-4893-a7fa-9bc8a0d767dd">

**Metáfora visual:**

Imagina miles de partículas flotando en un campo invisible Los graves son como olas pesadas que mueven partículas grandes y lentas. Los medios son como corrientes que mantienen el flujo. Los agudos son como ráfagas de viento que impulsan partículas pequeñas y rápidas.

</details>

---

<details>
<summary><strong>✏️ Diseños Conceptuales</strong></summary><br>

**Diagrama de Flujo del Sistema:**

Este diagrama muestra cómo el audio se transforma en visualización paso a paso.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FLUJO COMPLETO DEL SISTEMA                       │
└─────────────────────────────────────────────────────────────────────┘

PASO 1: ENTRADA DE AUDIO
┌──────────────────┐
│   🎵 AUDIO       │  ← Música (mp3, archivo cargado)
│   (Música)       │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ 📊 ANÁLISIS FFT  │  ← p5.FFT analiza frecuencias cada frame
│  + AMPLITUDE     │
└────────┬─────────┘
         │
         ├─────────────────────────────────────────┐
         │                                         │
         ▼                                         ▼
    FRECUENCIAS                               VOLUMEN
    ┌─────────┐                           ┌──────────┐
    │ BASS    │ 0-255 (20-140 Hz)         │ LEVEL    │ 0-1
    │ MID     │ 0-255 (140-2000 Hz)       │ (Amplitud│
    │ TREBLE  │ 0-255 (2000-20000 Hz)     │  general)│
    └────┬────┘                           └─────┬────┘
         │                                      │
         └──────────────┬───────────────────────┘
                        │
PASO 2: MAPEO A PARÁMETROS DEL FLOW FIELD
                        │
                        ▼
         ┌──────────────────────────┐
         │  PARÁMETROS MAPEADOS:    │
         │  • bass   → a1 (0.5-5)   │  ← Dirección horizontal
         │  • mid    → a2 (0.5-5)   │  ← Intensidad del campo
         │  • treble → a3 (0.5-5)   │  ← Rotaciones/remolinos
         │  • level  → a4 (0.5-3)   │  ← Variación del noise
         │  • level  → a5 (5-15)    │  ← Velocidad global
         └──────────────┬───────────┘
                        │
PASO 3: CÁLCULO DEL FLOW FIELD (en Mobile.update())
                        │
                        ▼
         ┌──────────────────────────┐
         │   FLOW FIELD (Común)     │
         │                          │
         │ velocity = createVector( │
         │   noise(a4 + a2*sin(...)),│  ← Usa a1, a2, a3, a4
         │   noise(a2 + a3*cos(...)) │     del audio
         │ )                        │
         │                          │
         │ velocity.mult(a5)        │  ← a5 controla velocidad
         │ velocity.rotate(...)     │  ← a3 controla rotación
         └──────────────┬───────────┘
                        │
                        │ HERENCIA: Todas las partículas heredan
                        │ este cálculo de flow field de Mobile
                        │
         ┌──────────────┴──────────────┐
         │                             │
PASO 4: POLIMORFISMO - REACCIONES ESPECÍFICAS POR TIPO
         │                             │
    ┌────▼─────┐   ┌────▼─────┐   ┌───▼──────┐
    │BassMobile│   │MidMobile │   │TrebleMob.│
    │          │   │          │   │          │
    │ 1000     │   │ 1000     │   │ 1000     │
    │partículas│   │partículas│   │partículas│
    └────┬─────┘   └────┬─────┘   └────┬─────┘
         │              │              │
         │              │              │
    SI bass>200    SI mid>180    SI treble>160
         │              │              │
         ▼              ▼              ▼
    • Más          • Cambian      • Se aceleran
      VISIBLES       de COLOR        2.5x
    • Más          • Verde →      • Partículas
      GRUESAS        Cian           más rápidas
    • Líneas       • Tono         • Energía
      1.5→3px        dinámico       de agudos
         │              │              │
         └──────────────┴──────────────┘
                        │
PASO 5: RENDERIZADO VISUAL
                        │
                        ▼
         ┌──────────────────────────┐
         │   CANVAS (800x800)       │
         │                          │
         │  🔴 Rojas/Naranjas       │ ← BassMobile (gruesas, lentas)
         │  🟢 Verdes/Azules        │ ← MidMobile (normales)
         │  🟣 Magentas             │ ← TrebleMobile (finas, rápidas)
         │                          │
         │  3000 partículas total   │
         │  Todas reaccionan al     │
         │  mismo flow field pero   │
         │  con comportamientos     │
         │  diferenciados           │
         └──────────────────────────┘
```


**Paleta de Colores:**

```
BASS (Graves):    Rojos/Naranjas  [HSB: 0-40]
                  Barras: Naranja brillante [HSB: 20, 255, 255]
                  
MID (Medios):     Verdes/Azules   [HSB: 120-200]
                  Barras: Verde brillante [HSB: 120, 255, 255]
                  
TREBLE (Agudos):  Magentas        [HSB: 280-360]
                  Barras: Magenta brillante [HSB: 300, 255, 255]
```

Las barras de frecuencia usan exactamente los mismos rangos de color que las partículas que representan, creando coherencia visual entre el análisis y la visualización.



</details>

---

<details>
<summary><strong>🎮 Diseño de Interacción</strong></summary><br>

**Entrada Principal: Audio**
- Análisis en tiempo real de frecuencias (Bass, Mid, Treble)
- Análisis de amplitud general

**Controles del Usuario:**

| Tecla/Control | Acción | Propósito |
|---------------|--------|-----------|
| **Botón** | Toggle UI | Mostrar/ocultar interfaz (funciona en móvil) |
| **SPACE** | Reset partículas | Reiniciar sin detener música |
| **P** | Play/Pause | Controlar reproducción |
| **B** | Toggle B/N | Modo blanco y negro  |
| **C** | Limpiar | Borrar rastros acumulados |
| **S** | Screenshot | Capturar momento único |
| **R** | Reiniciar todo | Comenzar desde cero |
| **U** | Toggle UI | Ocultar interfaz para contemplación (también vía botón) |
| **L** | Toggle Lifespan | Activar/desactivar muerte de partículas |
| **Slider Volumen** | Control de volumen | Ajustar volumen del audio de 0% a 100% |

**Selector de Archivo:**
- Input file debajo del canvas
- Permite cargar cualquier archivo de audio (mp3, wav, ogg)
- Cambia inmediatamente la canción que esta sonando

**Retroalimentación Visual:**

1. **Botón Toggle UI (esquina superior izquierda):**
   - Botón siempre visible
   - Permite mostrar/ocultar UI con un clic
   - Por defecto el UI está OCULTO para no abrumar al usuario
   - Funcional en dispositivos móviles sin teclado

2. **UI Informativo (visible al activar UI):**
   - FPS
   - Número de partículas activas
   - Estado de reproducción
   - Modo actual (Color/B&N)
   - Estado de Lifespan (**ON por defecto**)
   - Volumen actual
   - Nombre de la canción

3. **Barras de Frecuencia (esquina superior derecha):**
   - Barra BASS (Naranja) - Muestra energía de graves
   - Barra MID (Verde) - Muestra energía de medios
   - Barra TREB (Magenta) - Muestra energía de agudos
   - Valores numéricos debajo de cada barra

4. **Controles Adicionales (debajo del canvas):**
   - Selector de archivo de audio
   - Slider de volumen (0-100%)

5. **Instrucciones (parte inferior):**
   - Todos los controles disponibles
   - Siempre visible cuando UI está activo

</details>

---

<img width="1000" src="https://github.com/user-attachments/assets/da77ab3d-2420-41f7-b375-5d8d988affab">


### 💻 FASE 2: IMPLEMENTACIÓN TÉCNICA

<details>
<summary><strong>🏗️ Herencia y Polimorfismo - Conceptos de POO Aplicados</strong></summary><br>

Esta sección documenta cómo se aplicaron los conceptos de **Programación Orientada a Objetos (POO)** investigados en la fase de Seek, específicamente herencia y polimorfismo.


**Diagrama de Arquitectura (Herencia y Polimorfismo):**

```
┌─────────────────────────────────────────────────────────────┐
│              ESTRUCTURA DE CLASES (POO)                     │
└─────────────────────────────────────────────────────────────┘

                    ┌─────────────────┐
                    │     Mobile      │ ◄─── CLASE PADRE
                    │  (Clase Padre)  │
                    ├─────────────────┤
                    │ • position      │
                    │ • velocity      │
                    │ • lifespan      │
                    │ • mass = 1      │
                    │ • speedMult = 1 │
                    ├─────────────────┤
                    │ + run()         │ ◄─── Heredado por todas
                    │ + update()      │ ◄─── Lógica del flow field
                    │ + display()     │
                    │ + isDead()      │ ◄─── Heredado por todas
                    └────────┬────────┘
                             │
                 ┌───────────┼───────────┐
                 │           │           │
                 │ extends   │ extends   │ extends
                 │           │           │
         ┌───────▼──┐  ┌─────▼────┐  ┌──▼────────┐
         │BassMobile│  │MidMobile │  │TrebleMob. │ ◄─── CLASES HIJAS
         ├──────────┤  ├──────────┤  ├───────────┤
         │mass = 2  │  │mass = 1  │  │mass = 0.5 │ ◄─── SOBRESCRIBEN
         │speed=0.7 │  │speed = 1 │  │speed=1.5  │      propiedades
         │stroke=1.5│  │stroke=0.8│  │stroke=0.3 │
         │ROJA      │  │VERDE     │  │MAGENTA    │
         ├──────────┤  ├──────────┤  ├───────────┤
         │update()  │  │update()  │  │update()   │ ◄─── SOBRESCRIBEN
         │ +reacción│  │ +reacción│  │ +reacción │      comportamiento
         │ a BASS   │  │ a MID    │  │ a TREBLE  │
         └──────────┘  └──────────┘  └───────────┘
              │              │              │
              └──────────────┴──────────────┘
                             │
                    ARRAY POLIMÓRFICO
                             │
                             ▼
    mobiles[] = [Bass, Bass, ..., Mid, Mid, ..., Treble, Treble, ...]
                 │                                  │
                1000 objetos                       1000 objetos
                      33% + 33% + 34% = 100% (3000 total)
```

---

#### **Los 4 Pilares de POO Implementados**

**1. ENCAPSULACIÓN** 🔒

Cada clase encapsula sus propios datos y métodos que operan sobre esos datos.

Cuando definimoss propiedades dentro del constructor (`this.lifespan`, `this.mass`, etc.), cada objeto tiene su propia copia. Aunque técnicamente son accesibles desde fuera, la convención es que solo los métodos de la clase deberían modificarlas.

```javascript
class Mobile {
  constructor(index) {
    // Datos encapsulados (privados para cada instancia)
    this.lifespan = 255;
    this.mass = 1;
    this.speedMult = 1;
    this.position = createVector(random(width), random(height));
  }

  // Métodos que operan sobre datos encapsulados
  isDead() {
    return useLifespan && this.lifespan < 0;
  }
}
```

- ✅ Cada partícula mantiene su propio estado interno
- ✅ No se puede acceder directamente a `lifespan` desde fuera
- ✅ Solo se modifica a través de métodos como `update()`

---

**2. ABSTRACCIÓN** 🎭

La clase `Mobile` abstrae el comportamiento común de todas las partículas, ocultando detalles de implementación.

```javascript
// Uso simple desde sketch.js (abstracción):
mobiles[i].run(bass, mid, treble, level);

// Internamente ejecuta lógica compleja (oculta):
// - Cálculo de flow field con ruido Perlin
// - Aplicación de fuerzas y velocidades
// - Wrap-around en bordes
// - Envejecimiento de partícula
```

- ✅ El usuario de la clase no necesita saber cómo funciona el flow field
- ✅ Solo necesita llamar `run()` y la partícula se comporta correctamente

---

**3. HERENCIA** 🌳

Tres clases hijas heredan de la clase padre `Mobile`.

```javascript
// CLASE PADRE
class Mobile {
  constructor(index) {
    this.lifespan = 255;
    this.mass = 1;           // ← Será sobrescrito
    this.speedMult = 1;      // ← Será sobrescrito
  }

  run(bass, mid, treble, level) {  // ← HEREDADO sin cambios
    this.update(bass, mid, treble, level);
    this.display();
  }

  isDead() {  // ← HEREDADO sin cambios
    return useLifespan && this.lifespan < 0;
  }
}

// CLASE HIJA
class BassMobile extends Mobile {  // ← HERENCIA
  constructor(index) {
    super(index);  // ← Llama al constructor padre

    // SOBRESCRIBE propiedades para comportamiento específico
    this.mass = 2;              // Más pesadas
    this.speedMult = 0.7;       // Más lentas
    this.lifespanDecay = random(0.3, 1);  // Viven más tiempo
  }

  // SOBRESCRIBE + EXTIENDE update()
  update(bass, mid, treble, level) {
    super.update(bass, mid, treble, level);  // ← Ejecuta lógica padre

    // AGREGA comportamiento específico
    if (bass > 200) {
      this.trans += 5;
      this.strokeWeightVal = map(bass, 200, 255, 1.5, 3);
    }
  }
}
```

**¿Qué heredan las clases hijas?**
- ✅ Propiedades: `position`, `velocity`, `lifespan`, `acceleration`
- ✅ Métodos: `run()`, `isDead()`, lógica base de `update()`

**¿Qué sobrescriben?**
- ✅ Constructor: Valores específicos de `mass`, `speedMult`, `strokeWeightVal`
- ✅ `update()`: Agregan reacciones específicas a frecuencias
- ✅ `display()`: Colores y estilos específicos

---

**4. POLIMORFISMO** ⚡

Un solo array contiene objetos de diferentes tipos que responden diferente al mismo método.

```javascript
// CREACIÓN DE ARRAY POLIMÓRFICO (sketch.js)
let mobiles = [];

// Agregar diferentes tipos al mismo array
for (let i = 0; i < 1000; i++) {
  mobiles.push(new BassMobile(i));    // Tipo 1
}
for (let i = 1000; i < 2000; i++) {
  mobiles.push(new MidMobile(i));     // Tipo 2
}
for (let i = 2000; i < 3000; i++) {
  mobiles.push(new TrebleMobile(i));  // Tipo 3
}

// Resultado: mobiles = [Bass, Bass, ..., Mid, Mid, ..., Treble, Treble, ...]
```

**Ejecución polimórfica:**
```javascript
// MISMO método, DIFERENTES comportamientos
for (let i = 0; i < mobiles.length; i++) {
  mobiles[i].run(bass, mid, treble, level);  // ← Cada tipo ejecuta SU versión
  // BassMobile.run() → reacciona cuando bass > 200
  // MidMobile.run() → reacciona cuando mid > 180
  // TrebleMobile.run() → reacciona cuando treble > 160
}

// Mismo método isDead() funciona para TODOS los tipos
if (mobiles[i].isDead()) {
  mobiles.splice(i, 1);
}
```

---

#### **Tabla Comparativa: Investigación vs Implementación**

Comparación entre el ejemplo investigado (Ejemplo 4.5 - Particle System) y la implementación actual:

| Concepto | Fase Seek (Ejemplo 4.5) | Implementación (Flow Field Sónico) |
|----------|-------------------------|-------------------------------------|
| **Clase Padre** | `Particle` | `Mobile` |
| **Clases Hijas** | `Confetti extends Particle` | `BassMobile extends Mobile`<br>`MidMobile extends Mobile`<br>`TrebleMobile extends Mobile` |
| **Array Polimórfico** | `[Particle, Confetti, Particle, ...]` | `[BassMobile, MidMobile, TrebleMobile, ...]` |
| **Métodos Heredados** | `isDead()`, `update()` | `run()`, `isDead()`, lógica base de `update()` |
| **Métodos Sobrescritos** | `show()` (dibuja cuadrado vs círculo) | `update()` (reacciones a frecuencias)<br>`display()` (colores específicos) |
| **Diferenciación** | Visual (forma) | Visual (color) + Comportamental (reacción a audio) |
| **Proporción en Array** | 50% Particle / 50% Confetti | 33% Bass / 33% Mid / 34% Treble |

---

#### **Justificación: Ventajas de Usar Herencia**

**❌ SIN HERENCIA (código duplicado):**

Si no usáramos herencia, tendríamos que repetir todo el código del flow field en cada clase:

- `BassMobile`: ~130 líneas (cálculo completo de flow field + wrap-around + lifespan)
- `MidMobile`: ~130 líneas (mismo código repetido)
- `TrebleMobile`: ~130 líneas (mismo código repetido)
- **Total:** ~390 líneas con ~80% de código duplicado

**✅ CON HERENCIA (código reutilizable):**

Con herencia, separamos lo común de lo específico:

- `Mobile` (mobile.js): 94 líneas (lógica común del flow field)
- `BassMobile` (mobile_bass.js): 41 líneas (solo diferencias)
- `MidMobile` (mobile_mid.js): ~35 líneas (solo diferencias)
- `TrebleMobile` (mobile_treble.js): ~35 líneas (solo diferencias)
- **Total:** ~205 líneas

**Beneficios concretos:**

| Métrica | Sin Herencia | Con Herencia | Mejora |
|---------|--------------|--------------|--------|
| Líneas de código | ~390 | ~205 | **-47%** |
| Código duplicado | ~80% | 0% | **-100%** |
| Mantenibilidad | Baja (3 lugares) | Alta (1 lugar) | ✅ |
| Escalabilidad | Difícil | Fácil | ✅ |

- ✅ **47% menos código** (205 vs 390 líneas)
- ✅ **Mantenibilidad:** Cambios en el flow field solo se hacen en `Mobile.update()`
- ✅ **Escalabilidad:** Agregar un nuevo tipo (ej: `SubBassMobile`) requiere solo ~35 líneas
- ✅ **Consistencia:** Todas las partículas comparten la misma física base automáticamente
- ✅ **Debugging:** Errores en el flow field se corrigen una sola vez

</details>

---

<details>
<summary><strong>📊 Conceptos de las 4 Unidades Anteriores Aplicados</strong></summary><br>

<details>
<summary><strong>Unidad 1: Aleatoriedad - Ruido Perlin</strong></summary><br>

**¿Dónde?** En el cálculo del Flow Field

```javascript
// En Mobile.update()
this.velocity = createVector(
  1 - 2 * noise(a4 + a2 * sin(TAU * this.position.x / width),
                a4 + a2 * sin(TAU * this.position.y / height)),
  1 - 2 * noise(a2 + a3 * cos(TAU * this.position.x / width),
                a4 + a3 * cos(TAU * this.position.y / height))
);
```

**¿Cómo funciona?**
- Usa `noise()` para generar valores suaves entre 0 y 1
- Crea un campo vectorial continuo (flow field)
- Las partículas siguen el campo de manera orgánica
- `a1, a2, a3, a4` se mapean desde el audio, haciendo el campo reactivo

**¿Por qué?**
- `random()` crearía movimiento caótico y errático
- `noise()` crea patrones fluidos como agua o viento
- Es la base de los flow fields naturales

</details>

<details>
<summary><strong>Unidad 2: Vectores - Operaciones Vectoriales</strong></summary><br>

**¿Dónde?** En toda la física del movimiento

```javascript
// Velocidad como vector 2D
this.velocity = createVector(x, y);

// Posición actualizada con suma vectorial
this.position.add(this.velocity);

// Rotación vectorial
this.velocity.rotate(sin(100) * noise(...));

// Magnitud para wrap-around inteligente
let distance = dist(pos0.x, pos0.y, pos.x, pos.y);
```

**¿Cómo funciona?**
- Cada partícula tiene posición y velocidad como vectores
- Se actualizan con operaciones vectoriales (`add`, `rotate`, `mult`)
- Permite movimiento suave en 2D

**¿Por qué?**
- Los vectores representan dirección y magnitud simultáneamente
- Facilitan cálculos de física
- Permiten rotaciones y transformaciones elegantes

</details>

<details>
<summary><strong>Unidad 3: Fuerzas - Masa y Reacción Diferenciada</strong></summary><br>

**¿Dónde?** En las propiedades de cada tipo de partícula

```javascript
// BassMobile
this.mass = 2;           // Más pesada
this.speedMult = 0.7;    // Más lenta

// MidMobile  
this.mass = 1;           // Normal

// TrebleMobile
this.mass = 0.5;         // Más ligera
this.speedMult = 1.5;    // Más rápida
```

**¿Cómo funciona?**
- Partículas con mayor `mass` reaccionan más lentamente
- `speedMult` escala la velocidad final
- Diferentes tipos responden diferente a las mismas fuerzas del flow field

**¿Por qué?**
- Simula inercia: graves = pesados, agudos = ligeros
- Crea variedad visual según la frecuencia
- Representa metafóricamente el "peso" del sonido

</details>

<details>
<summary><strong>Unidad 4: Oscilaciones - Funciones Trigonométricas</strong></summary><br>

**¿Dónde?** En el cálculo del flow field y rotación

```javascript
// Uso de seno y coseno en el flow field
sin(TAU * this.position.x / width)
cos(TAU * this.position.y / height)

// Rotación de velocidad
this.velocity.rotate(sin(100) * noise(...));
```

**¿Cómo funciona?**
- `sin()` y `cos()` crean patrones ondulantes
- `TAU` (2π) permite ciclos completos
- La rotación añade remolinos al movimiento

**¿Por qué?**
- Las funciones trigonométricas crean movimientos circulares/ondulantes
- Son fundamentales para flow fields orgánicos
- Añaden complejidad visual sin caos

</details><br>

</details>

---

<details>
<summary><strong>🔄 Gestión de Tiempo de Vida y Memoria</strong></summary><br>

**Sistema de Lifespan (ON por defecto - Toggle con L o botón):**

```javascript
// Cada partícula tiene:
this.lifespan = 255;                    // Empieza en 255 vida y full color
this.lifespanDecay = random(0.5, 2.5);  // Velocidad de muerte

// En update():
if (useLifespan) {
  this.lifespan -= this.lifespanDecay;
}

// En display():
let alpha = useLifespan ? this.lifespan : this.trans;
```

**Gestión de Memoria:**

**Modo 1: Con Lifespan (DEFAULT - Demuestra gestión de memoria de Unidad 5)**
```javascript
// Eliminar partículas muertas
if (useLifespan && mobiles[i].isDead()) {
  mobiles.splice(i, 1);  // Liberar memoria
}

// Reponer automáticamente
if (useLifespan && mobiles.length < nmobiles) {
  let needed = nmobiles - mobiles.length;
  // Crear nuevas partículas para mantener ~3000 activas
  for (let i = 0; i < needed; i++) {
    // Crear proporcionalmente Bass/Mid/Treble mobiles
  }
}
```
- **Por defecto desde el inicio** para demostrar los conceptos de la Unidad 5
- Partículas mueren gradualmente y son reemplazadas
- Array dinámico: `mobiles.length` fluctúa cerca de 3000
- Demuestra uso de `splice()` para liberar memoria una vez mueren
- Ciclo de vida continuo: nacimiento → vida → muerte → renacimiento

**Modo 2: Sin Lifespan (Toggle L para desactivar)**
- 3000 partículas permanentes e inmortales
- Nunca mueren, nunca se eliminan
- Array estable: `mobiles.length` siempre = 3000
- Óptimo para rendimiento (sin overhead de crear/eliminar)

</details>

---

<details>
<summary><strong>🔄 CICLO DE VIDA de una particula</strong></summary><br>

Esta sección documenta **técnicamente** cómo nace, vive, envejece, muere y renace una partícula.

#### **1️⃣ NACIMIENTO** (Constructor)

**Ubicación en código:** `mobile.js:3-21`

```javascript
constructor(index) {
  // Inicialización de propiedades básicas...

  // 🎂 Nacimiento con vida máxima
  this.lifespan = 255;                    // ← NACE con vida completa (opacidad 100%)
  this.lifespanDecay = random(0.5, 2);    // ← Velocidad de envejecimiento aleatoria
}
```

**Valores iniciales:**
- `lifespan = 255` (vida máxima, totalmente visible)
- `lifespanDecay = random(0.5, 2)` (cada partícula envejece a diferente velocidad)

**Variación por tipo de partícula:**
```javascript
// BassMobile - Partículas LONGEVAS
this.lifespanDecay = random(0.3, 1);  // Mueren 2-3x más lento (representan graves "pesados")

// MidMobile y TrebleMobile - Partículas NORMALES
this.lifespanDecay = random(0.5, 2);  // Velocidad estándar
```

**Duración de vida esperada (a 60 FPS):**
- **BassMobile**: 255 ÷ random(0.3, 1) = **255 a 850 frames** = **4.2 a 14 segundos**
- **Mid/TrebleMobile**: 255 ÷ random(0.5, 2) = **127 a 510 frames** = **2 a 8.5 segundos**

---

#### **2️⃣ VIDA Y ENVEJECIMIENTO** (Update)

**Ubicación en código:** `mobile.js:64-67`

```javascript
update(bass, mid, treble, level) {
  // ... cálculos de física y movimiento ...

  // ⏳ Envejecimiento gradual
  if (useLifespan) {
    this.lifespan -= this.lifespanDecay;  // ← ENVEJECE cada frame (60 veces/segundo)
  }
}
```

**Proceso de envejecimiento:**
- Cada frame (1/60 de segundo), `lifespan` disminuye por `lifespanDecay`
- Si `lifespanDecay = 1`, pierde **1 punto de vida** por frame
- Si `lifespanDecay = 2`, pierde **2 puntos de vida** por frame (muere el doble de rápido)

**Ejemplo de progresión temporal:**
```
Frame 0:    lifespan = 255   (recién nacida, opaca)
Frame 1:    lifespan = 254   (si decay = 1)
Frame 60:   lifespan = 195   (1 segundo de vida)
Frame 127:  lifespan = 128   (mitad de vida, 50% transparente)
Frame 254:  lifespan = 1     (casi muerta, casi invisible)
Frame 255:  lifespan = 0     (muerta, invisible)
Frame 256:  lifespan = -1    (MARCADA PARA ELIMINACIÓN)
```

---

#### **3️⃣ VISUALIZACIÓN** (Display)

**Ubicación en código:** `mobile.js:70-88`

```javascript
display() {
  // 🎨 El lifespan controla la transparencia (alpha)
  let alpha = useLifespan ? this.lifespan : this.trans;

  // Dibujar línea con opacidad = lifespan
  stroke(this.hu, this.sat, this.bri, alpha);  // ← Alpha varía de 255 a 0
  line(this.position0.x, this.position0.y, this.position.x, this.position.y);
}
```

**Efecto visual del envejecimiento:**
- `lifespan = 255` → Partícula **totalmente visible** (opacidad 100%)
- `lifespan = 192` → Partícula **visible** (opacidad 75%)
- `lifespan = 128` → Partícula **semi-transparente** (opacidad 50%)
- `lifespan = 64` → Partícula **muy transparente** (opacidad 25%)
- `lifespan = 0` → Partícula **invisible** (opacidad 0%)

**Resultado:** Las partículas se **desvanecen gradualmente** antes de morir, creando un efecto visual suave de extinción.

---

#### **4️⃣ MUERTE** (isDead)

**Ubicación en código:** `mobile.js:90-93`

```javascript
isDead() {
  return useLifespan && this.lifespan < 0;  // ← Muere cuando cruza el umbral de 0
}
```

**Condición de muerte:**
- Si `useLifespan = true` **Y** `lifespan < 0` → La partícula está **oficialmente muerta**
- Nota: Cuando `lifespan = 0` aún está "viva" pero invisible
- Muere realmente cuando `lifespan` se vuelve **negativo**

---

#### **5️⃣ ELIMINACIÓN Y RENACIMIENTO** (Draw Loop)

**Ubicación en código:** `sketch.js:155-182`

```javascript
// ⚰️ ELIMINACIÓN: Recorrer array AL REVÉS para poder eliminar sin problemas
for (let i = mobiles.length - 1; i >= 0; i--) {
  mobiles[i].run(bass, mid, treble, level);

  // Si está muerta, ELIMINAR del array
  if (useLifespan && mobiles[i].isDead()) {
    mobiles.splice(i, 1);  // ← LIBERA MEMORIA (eliminación física del array)
  }
}

// 🌱 RENACIMIENTO: Reponer partículas eliminadas
if (useLifespan && mobiles.length < nmobiles) {
  let needed = nmobiles - mobiles.length;  // Calcular cuántas faltan

  for (let i = 0; i < needed; i++) {
    let r = random(1);
    // Crear nuevas partículas en proporciones iguales (33% cada tipo)
    if (r < 0.33) {
      mobiles.push(new BassMobile(mobiles.length));  // ← NACE nueva partícula Bass
    } else if (r < 0.66) {
      mobiles.push(new MidMobile(mobiles.length));   // ← NACE nueva partícula Mid
    } else {
      mobiles.push(new TrebleMobile(mobiles.length)); // ← NACE nueva partícula Treble
    }
  }
}
```

**Proceso completo de eliminación y renacimiento:**

1. **Detección**: `isDead()` retorna `true` cuando `lifespan < 0`
2. **Eliminación**: `splice(i, 1)` remueve la partícula del array
3. **Liberación de memoria**: JavaScript garbage collector eventualmente libera la memoria
4. **Detección de faltantes**: Se compara `mobiles.length < 3000`
5. **Cálculo**: `needed = 3000 - mobiles.length` (ej: si quedan 2997, needed = 3)
6. **Creación**: Se crean `needed` partículas nuevas en proporciones balanceadas
7. **Renacimiento**: Nuevas partículas nacen con `lifespan = 255` (vuelve al paso 1 ⟲)

---

#### **📊 DIAGRAMA DE FLUJO DEL CICLO DE VIDA**

```
┌───────────────────────────────────────────────────────────────┐
│                  CICLO DE VIDA DE UNA PARTÍCULA               │
└───────────────────────────────────────────────────────────────┘

    ┌─────────────────┐
    │  1. NACIMIENTO  │  Constructor: lifespan = 255
    └────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │  2. VIDA        │  Cada frame: lifespan -= decay
    │  (255 → 0)      │  Se mueve según flow field
    └────────┬────────┘  Dura 2-14 segundos
             │
             ▼
    ┌─────────────────┐
    │  3. VISUAL      │  display(): alpha = lifespan
    │  (Desvanece)    │  255 (opaco) → 0 (invisible)
    └────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │  4. MUERTE      │  isDead(): lifespan < 0
    └────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │  5. ELIMINACIÓN │  splice(i, 1)
    │  (Libera RAM)   │  mobiles.length disminuye
    └────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │  6. RENACIMIENTO│  new Mobile()
    │  (Reposición)   │  Mantener ~3000 activas
    └────────┬────────┘
             │
             └──────────────┐
                            │
                            ▼
                     Vuelve al paso 1 ⟲
```

---

#### **🔬 VALORES CLAVE**

| Variable | Ubicación | Rango | Significado |
|----------|-----------|-------|-------------|
| `this.lifespan` | Mobile class | 255 → -1 | Vida actual de la partícula |
| `this.lifespanDecay` | Mobile class | 0.3 - 2 | Velocidad de envejecimiento |
| `mobiles.length` | sketch.js | ~3000 | Partículas activas en el sistema |
| `useLifespan` | sketch.js | true/false | Sistema activado/desactivado |

**Comportamiento dinámico del array:**
- Con Lifespan ON: `mobiles.length` fluctúa (ej: 2995 → 2998 → 3001 → 2997)
- Con Lifespan OFF: `mobiles.length` = 3000 (constante)

**Observación en tiempo real:**
- Puedes ver el número de partículas activas en el UI: `Partículas: ${mobiles.length}`
- El número fluctúa constantemente demostrando el ciclo de muerte/renacimiento

---

**Estrategia de Wrap-Around:**
```javascript
// Ajustar AMBAS posiciones al cruzar bordes
if (this.position.x > width) {
  this.position.x -= width;
  this.position0.x -= width;  // ← Clave para evitar líneas atravesadas
}
```
- Previene líneas que atraviesan el canvas
- Mantiene continuidad visual

</details>

---

<details>
<summary><strong>🎵 Análisis de Audio en Tiempo Real</strong></summary><br>

**Setup de Audio:**

```javascript
fft = new p5.FFT(0.8, 512);      // Smoothing, bins
amplitude = new p5.Amplitude();

// Conectar al audio
fft.setInput(song);
amplitude.setInput(song);
```

**En cada frame:**

```javascript
fft.analyze();  // ← CRÍTICO: analizar primero

let bass = fft.getEnergy("bass");      // 0-255 (20-140 Hz)
let mid = fft.getEnergy("mid");        // 0-255 (140-2000 Hz)
let treble = fft.getEnergy("treble");  // 0-255 (2000-20000 Hz)
let level = amplitude.getLevel();      // 0-1 (volumen general)

// Mapear a parámetros del flow field
a1 = map(bass, 0, 255, 0.5, 5);     // Bass → a1
a2 = map(mid, 0, 255, 0.5, 5);      // Mid → a2
a3 = map(treble, 0, 255, 0.5, 5);   // Treble → a3
a4 = map(level, 0, 0.5, 0.5, 3);    // Level → a4
a5 = map(level, 0, 1, 5, 15);       // Level → velocidad global
```

</details>

---

<details>
<summary><strong>📊 Cómo cada frecuencia afecta el flow field</strong></summary>

Esta sección explica **técnicamente** cómo el audio controla el comportamiento visual de las partículas.

<details>
<summary><strong>1️⃣ BASS (Graves) → Parámetro `a1`</strong></summary><br>

**Rango de frecuencias:** 20-140 Hz (sonidos profundos como kicks, bajo, tambores graves)

**Qué controla en el código:**
```javascript
// En Mobile.update() - Componente X de la velocidad
1 - 2 * noise(a4 + a2 * sin(TAU * this.position.x / width), ...)
```

**Efecto visual:**
- Controla la **dirección horizontal** del campo vectorial
- A mayor bass → mayor variación en `a1` → el flow field cambia más dramáticamente en el eje X
- Las partículas cambian de dirección horizontal de forma más pronunciada

**Reacción específica de BassMobile:**
```javascript
// Cuando bass > 200 (graves muy fuertes)
if (bass > 200) {
  this.trans += 5;                              // Más visibles (transparencia aumenta)
  this.strokeWeight = map(bass, 200, 255, 1.5, 3);  // Líneas más GRUESAS
}
```
- Las partículas rojas/naranjas se vuelven **más brillantes y más gruesas** cuando hay graves fuertes
- Representan visualmente el "peso" del bass

</details>

<details>
<summary><strong>2️⃣ MID (Medios) → Parámetro `a2`</strong></summary><br>

**Rango de frecuencias:** 140-2000 Hz (voces, guitarras, la mayoría de melodías)

**Qué controla en el código:**
```javascript
// En Mobile.update() - Intensidad del campo
a2 * sin(TAU * this.position.x / width)
a2 * sin(TAU * this.position.y / height)
```

**Efecto visual:**
- Controla la **intensidad y complejidad** del campo vectorial
- A mayor mid → mayor `a2` → el campo se vuelve más denso y complejo
- Las ondas sinusoidales tienen mayor amplitud, creando patrones más elaborados

**Reacción específica de MidMobile:**
```javascript
// Cuando mid > 180
if (mid > 180) {
  this.hu = map(mid, 180, 255, 120, 200);  // Cambian de COLOR dinámicamente
}
```
- Las partículas verdes/azules **cambian de tono** según la intensidad de los medios
- Del verde (120°) al azul cian (200°) en el espectro HSB

</details>

<details>
<summary><strong>3️⃣ TREBLE (Agudos) → Parámetro `a3`</strong></summary><br>

**Rango de frecuencias:** 2000-20000 Hz (platillos, hi-hats, brillos, notas agudas)

**Qué controla en el código:**
```javascript
// En Mobile.update() - Rotación del campo
this.velocity.rotate(sin(100) * noise(a4 + a3 * sin(TAU * this.position.x / width)));

// Y en el componente Y de la velocidad
a3 * cos(TAU * this.position.x / width)
a3 * cos(TAU * this.position.y / height)
```

**Efecto visual:**
- Controla las **rotaciones y remolinos** del campo vectorial
- A mayor treble → mayor `a3` → más espirales y giros en el movimiento
- Las partículas crean patrones circulares más pronunciados

**Reacción específica de TrebleMobile:**
```javascript
// Cuando treble > 160
if (treble > 160) {
  this.speedMult = map(treble, 160, 255, 1.5, 2.5);  // Se ACELERAN
}
```
- Las partículas magentas se vuelven **hasta 2.5x más rápidas**
- Representan la energía y rapidez de los agudos

</details>

<details>
<summary><strong>4️⃣ LEVEL (Volumen general) → Parámetros `a4` + `a5`</strong></summary><br>

**Qué mide:** Amplitud general del audio (volumen total)

**Qué controla en el código:**
```javascript
// a4: Usado en el cálculo base del noise
noise(a4 + a2 * sin(...), a4 + a2 * sin(...))

// a5: Multiplicador de velocidad GLOBAL
this.velocity.mult(a5 * this.speedMult);
```

**Efecto visual:**
- `a4` añade variación constante al campo de ruido
- `a5` controla la **velocidad global** de TODAS las partículas
- A mayor volumen → todas las partículas se mueven más rápido
- Rango: de 5 (silencio) a 15 (volumen máximo)

</details>

---

**🎨 RESUMEN: Mapeo Audio → Visual**

| Frecuencia | Parámetro | Controla | Efecto Visual |
|------------|-----------|----------|---------------|
| **BASS** | `a1` | Dirección horizontal | Cambios dramáticos en eje X |
| **MID** | `a2` | Intensidad del campo | Patrones más densos y complejos |
| **TREBLE** | `a3` | Rotaciones | Remolinos y espirales |
| **LEVEL** | `a4`, `a5` | Variación + Velocidad | Movimiento general más rápido |

**Ejemplo de flujo completo:**
1. Una canción con **graves fuertes** → `a1` aumenta → Las BassMobile (rojas) se vuelven gruesas y el campo cambia en X
2. **Melodía intensa** → `a2` aumenta → Las MidMobile (verdes) cambian de color y el campo es más complejo
3. **Platillos/hi-hats** → `a3` aumenta → Las TrebleMobile (magentas) se aceleran y el campo rota más
4. **Volumen alto** → `a5` aumenta → Todas se mueven más rápido

</details>

---

<details>
    <summary><strong>💻 Código fuente completo</strong></summary><br>


<details>
    <summary>sketch.js</summary><br>

```js
let song;
let fft;
let amplitude;
let fileInput;
let songName = "Poshlaya molly - non stop";
let volumeSlider;
let toggleUIButton;

let nmobiles = 3000;
let mobiles = [];
let noisescale;
let a1, a2, a3, a4, a5, amax;
let bw = false;
let showUI = false;
let useLifespan = true;

function preload() {
  song = loadSound('Poshlaya molly - non stop.mp3', () => {
    console.log('✓ Archivo de audio cargado en preload');
  }, (err) => {
    console.error('✌ Error cargando audio:', err);
  });
}

function setup() {
  createCanvas(800, 800);
  background(0);
  noFill();
  colorMode(HSB, 360, 255, 255, 255);
  strokeWeight(0.5);
  fft = new p5.FFT(0.8, 512);
  amplitude = new p5.Amplitude();
  setupFileInput();
  setupVolumeSlider();
  setupToggleUIButton();
  if (song && song.isLoaded()) {
    fft.setInput(song);
    amplitude.setInput(song);
    song.setVolume(0.5);
    song.loop();
    console.log('✓ FFT conectado y música iniciada');
  } else {
    console.log('⚠ Esperando que el audio termine de cargar...');
  }
  reset();
}

function draw() {

  background(0, 10);
  
  if (song && song.isPlaying()) {
    
    if (frameCount < 10) {
      fft.setInput(song);
      amplitude.setInput(song);
    }
    fft.analyze();
    let bass = fft.getEnergy("bass");
    let mid = fft.getEnergy("mid");
    let treble = fft.getEnergy("treble");
    let level = amplitude.getLevel();
    a1 = map(bass, 0, 255, 0.5, 5);
    a2 = map(mid, 0, 255, 0.5, 5);
    a3 = map(treble, 0, 255, 0.5, 5);
    a4 = map(level, 0, 0.5, 0.5, 3);
    a5 = map(level, 0, 1, 5, 15);
    for (let i = mobiles.length - 1; i >= 0; i--) {
      mobiles[i].run(bass, mid, treble, level);
      if (useLifespan && mobiles[i].isDead()) {
        mobiles.splice(i, 1);
      }
    }
    if (useLifespan && mobiles.length < nmobiles) {
      let needed = nmobiles - mobiles.length;
      for (let i = 0; i < needed; i++) {
        let r = random(1);
        if (r < 0.33) {
          mobiles.push(new BassMobile(mobiles.length));
        } else if (r < 0.66) {
          mobiles.push(new MidMobile(mobiles.length));
        } else {
          mobiles.push(new TrebleMobile(mobiles.length));
        }
      }
    }
    
    if (showUI) {
      drawUI(bass, mid, treble, level);
      drawFrequencyBars(bass, mid, treble);
    }
    
  } else {
    
    if (showUI) {
      drawUI(0, 0, 0, 0);
    }
  }
}


function setupFileInput() {
  fileInput = createFileInput(handleFile);
  fileInput.position(10, height + 10);
  fileInput.style('color', '#fff');
  fileInput.style('background-color', '#333');
  fileInput.style('border', '1px solid #666');
  fileInput.style('padding', '5px');
}

function setupVolumeSlider() {
  volumeSlider = createSlider(0, 100, 50, 1);
  volumeSlider.position(10, height + 50);
  volumeSlider.style('width', '150px');
  volumeSlider.input(() => {
    if (song) {
      song.setVolume(volumeSlider.value() / 100);
    }
  });
}

function setupToggleUIButton() {
  toggleUIButton = createButton('👁');
  toggleUIButton.position(15, 15);
  toggleUIButton.size(40, 40);
  toggleUIButton.style('font-size', '20px');
  toggleUIButton.style('background-color', 'rgba(0, 0, 0, 0.7)');
  toggleUIButton.style('color', '#fff');
  toggleUIButton.style('border', '2px solid rgba(255, 255, 255, 0.3)');
  toggleUIButton.style('border-radius', '8px');
  toggleUIButton.style('cursor', 'pointer');
  toggleUIButton.style('transition', 'all 0.3s');
  toggleUIButton.mouseOver(() => {
    toggleUIButton.style('background-color', 'rgba(50, 50, 50, 0.9)');
    toggleUIButton.style('border-color', 'rgba(255, 255, 255, 0.6)');
  });
  toggleUIButton.mouseOut(() => {
    toggleUIButton.style('background-color', 'rgba(0, 0, 0, 0.7)');
    toggleUIButton.style('border-color', 'rgba(255, 255, 255, 0.3)');
  });
  toggleUIButton.mousePressed(() => {
    showUI = !showUI;
  });
}

function handleFile(file) {
  if (file.type === 'audio') {
    console.log('Cargando nueva canción...');
    songName = file.name.replace(/\.[^/.]+$/, "");
    if (song && song.isPlaying()) {
      song.stop();
    }
    song = loadSound(file.data, () => {
      console.log('✓ Nueva canción cargada y reproduciéndose');
      song.loop();
      song.setVolume(0.5);

      fft.setInput(song);
      amplitude.setInput(song);
      console.log('✓ FFT y Amplitude conectados a la nueva canción');
    });
  } else {
    console.log('⚠ Por favor sube un archivo de audio válido');
    alert('Por favor selecciona un archivo de audio (mp3, wav, ogg)');
  }
}

function reset() {
  noisescale = random(0.08, 0.1);
  noiseDetail(int(random(1, 5)));
  amax = random(5);
  a1 = random(1, amax);
  a2 = random(1, amax);
  a3 = random(1, amax);
  a4 = random(1, amax);
  a5 = 10;
  mobiles = [];
  let third = floor(nmobiles / 3);
  for (let i = 0; i < third; i++) {
    mobiles.push(new BassMobile(i));
  }
  for (let i = third; i < third * 2; i++) {
    mobiles.push(new MidMobile(i));
  }
  for (let i = third * 2; i < nmobiles; i++) {
    mobiles.push(new TrebleMobile(i));
  }
  console.log('Reset completado');
}

function drawUI(bass, mid, treble, level) {
  push();
  fill(0, 180);
  noStroke();
  rect(65, 5, 280, 120, 5);
  fill(255);
  textSize(12);
  textAlign(LEFT);
  text(`FPS: ${floor(frameRate())}`, 70, 20);
  text(`Partículas: ${mobiles.length}`, 70, 35);
  text(`Música: ${song && song.isPlaying() ? 'Play ▶' : 'Pause ⏸'}`, 70, 50);
  text(`Modo: ${bw ? 'B/N' : 'Color'}`, 70, 65);
  text(`Lifespan: ${useLifespan ? 'ON' : 'OFF'}`, 70, 80);
  text(`Volumen: ${volumeSlider.value()}%`, 70, 95);
  fill(100, 255, 255);
  textSize(11);
  let displayName = songName.length > 30 ? songName.substring(0, 30) + "..." : songName;
  text(`♪ ${displayName}`, 70, 110);
  fill(0, 180);
  rect(5, height - 60, width - 10, 55, 5);

  fill(255);
  textSize(11);
  text('CONTROLES:', 10, height - 45);
  text('SPACE: Reset | P: Pausa | B: B/N | C: Limpiar | S: Screenshot', 10, height - 30);
  text('U: Toggle UI | L: Toggle Lifespan | R: Reiniciar Todo', 10, height - 15);

  pop();
}

function drawFrequencyBars(bass, mid, treble) {
  push();
  
  let x = width - 140;
  let y = 20;
  let barWidth = 35;
  let barMaxHeight = 120;
  
  fill(0, 200);
  noStroke();
  rect(x - 10, y - 10, 140, barMaxHeight + 50, 5);
  let bassHeight = max(map(bass, 0, 255, 0, barMaxHeight), 2);
  let midHeight = max(map(mid, 0, 255, 0, barMaxHeight), 2);
  let trebleHeight = max(map(treble, 0, 255, 0, barMaxHeight), 2);
  fill(20, 255, 255, 220);
  noStroke();
  rect(x, y + barMaxHeight - bassHeight, barWidth, bassHeight, 2);
  noFill();
  stroke(20, 255, 255, 150);
  strokeWeight(2);
  rect(x, y, barWidth, barMaxHeight, 2);
  fill(20, 255, 255);
  noStroke();
  textSize(11);
  textAlign(CENTER);
  text('BASS', x + barWidth/2, y + barMaxHeight + 18);
  textSize(10);
  fill(255);
  text(floor(bass), x + barWidth/2, y + barMaxHeight + 32);
  fill(120, 255, 255, 220);
  noStroke();
  rect(x + 45, y + barMaxHeight - midHeight, barWidth, midHeight, 2);
  noFill();
  stroke(120, 255, 255, 150);
  strokeWeight(2);
  rect(x + 45, y, barWidth, barMaxHeight, 2);
  fill(120, 255, 255);
  noStroke();
  textSize(11);
  text('MID', x + 45 + barWidth/2, y + barMaxHeight + 18);
  textSize(10);
  fill(255);
  text(floor(mid), x + 45 + barWidth/2, y + barMaxHeight + 32);
  fill(300, 255, 255, 220);
  noStroke();
  rect(x + 90, y + barMaxHeight - trebleHeight, barWidth, trebleHeight, 2);
  noFill();
  stroke(300, 255, 255, 150);
  strokeWeight(2);
  rect(x + 90, y, barWidth, barMaxHeight, 2);
  fill(300, 255, 255);
  noStroke();
  textSize(11);
  text('TREB', x + 90 + barWidth/2, y + barMaxHeight + 18);
  textSize(10);
  fill(255);
  text(floor(treble), x + 90 + barWidth/2, y + barMaxHeight + 32);
  pop();
}

function keyPressed() {
  if (keyCode === 32) {
    reset();
    return false;
  }
  if (key === 'p' || key === 'P') {
    if (song) {
      if (song.isPlaying()) {
        song.pause();
      } else {
        song.play();
      }
    }
  }
  if (key === 'b' || key === 'B') {
    bw = !bw;
  }
  if (key === 'c' || key === 'C') {
    background(0);
  }
  if (key === 's' || key === 'S') {
    saveCanvas("FlowFieldSonico_" + year() + month() + day() + "_" + hour() + minute() + second() + ".png");
    console.log('Screenshot guardado');
  }
  if (key === 'r' || key === 'R') {
    if (song) {
      song.stop();
      song.loop();
    }
    background(0);
    reset();
  }
  if (key === 'u' || key === 'U') {
    showUI = !showUI;
  }
  if (key === 'l' || key === 'L') {
    useLifespan = !useLifespan;
    console.log('Lifespan:', useLifespan);
  }
  return false;
}
```

</details>

<details>
    <summary>mobile.js</summary><br>

```js
// MOBILE (Clase Padre)
class Mobile {
  constructor(index) {
    this.index = index;
    this.velocity = createVector(0, 0);
    this.position0 = createVector(random(width), random(height));
    this.position = this.position0.copy();
    this.trans = random(50, 100);
    this.hu = (noise(a1 * cos(PI * this.position.x / width), a1 * sin(PI * this.position.y / height)) * 720) % 360;
    this.sat = noise(a2 * sin(PI * this.position.x / width), a2 * sin(PI * this.position.y / height)) * 255;
    this.bri = noise(a3 * cos(PI * this.position.x / width), a3 * cos(PI * this.position.y / height)) * 255;
    this.mass = 1;
    this.speedMult = 1;
    this.strokeWeightVal = 0.5;
    this.lifespan = 255;
    this.lifespanDecay = random(0.5, 2);
  }
  run(bass, mid, treble, level) {
    this.update(bass, mid, treble, level);
    this.display();
  }
  update(bass, mid, treble, level) {
    this.velocity = createVector(
      1 - 2 * noise(a4 + a2 * sin(TAU * this.position.x / width),
                    a4 + a2 * sin(TAU * this.position.y / height)),
      1 - 2 * noise(a2 + a3 * cos(TAU * this.position.x / width),
                    a4 + a3 * cos(TAU * this.position.y / height))
    );
    this.velocity.mult(a5 * this.speedMult);
    this.velocity.rotate(sin(100) * noise(a4 + a3 * sin(TAU * this.position.x / width)));
    this.position0 = this.position.copy();
    this.position.add(this.velocity);
    if (this.position.x > width) {
      this.position.x = this.position.x - width;
      this.position0.x = this.position0.x - width;
    }
    if (this.position.x < 0) {
      this.position.x = this.position.x + width;
      this.position0.x = this.position0.x + width;
    }
    if (this.position.y > height) {
      this.position.y = this.position.y - height;
      this.position0.y = this.position0.y - height;
    }
    if (this.position.y < 0) {
      this.position.y = this.position.y + height;
      this.position0.y = this.position0.y + height;
    }
    if (useLifespan) {
      this.lifespan -= this.lifespanDecay;
    }
  }
  display() {
    let alpha = useLifespan ? this.lifespan : this.trans;
    let distance = dist(this.position0.x, this.position0.y, 
                       this.position.x, this.position.y);
    if (distance < 50) {
      if (bw) {
        stroke(255, alpha);
      } else {
        stroke(this.hu, this.sat, this.bri, alpha);
      }
      strokeWeight(this.strokeWeightVal);
      line(this.position0.x, this.position0.y, this.position.x, this.position.y);
    }
  }
  isDead() {
    return useLifespan && this.lifespan < 0;
  }
}
```

</details>

<details>
    <summary>mobile_bass.js</summary><br>

```js
// BASS (Graves)
class BassMobile extends Mobile {
  constructor(index) {
    super(index);
    this.mass = 2;
    this.speedMult = 0.7;
    this.strokeWeightVal = 1.5;
    this.trans = random(100, 150);
    this.hu = random(0, 40);
    this.lifespanDecay = random(0.3, 1);
  }
  update(bass, mid, treble, level) {
    super.update(bass, mid, treble, level);
    if (bass > 200) {
      this.trans = constrain(this.trans + 5, 100, 255);
      this.strokeWeightVal = map(bass, 200, 255, 1.5, 3);
    } else {
      this.trans = lerp(this.trans, 120, 0.05);
      this.strokeWeightVal = lerp(this.strokeWeightVal, 1.5, 0.05);
    }
  }
  display() {
    let alpha = useLifespan ? this.lifespan : this.trans;
    let distance = dist(this.position0.x, this.position0.y, 
                       this.position.x, this.position.y);
    if (distance < 50) {
      if (bw) {
        stroke(255, alpha);
      } else {
        stroke(this.hu, 200, 255, alpha);
      }
      strokeWeight(this.strokeWeightVal);
      line(this.position0.x, this.position0.y, this.position.x, this.position.y);
    }
  }
}
```

</details>

<details>
    <summary>mobile_mid.js</summary><br>

```js
// MID (Medios)
class MidMobile extends Mobile {
  constructor(index) {
    super(index);
    this.mass = 1;
    this.speedMult = 1;
    this.strokeWeightVal = 0.8;
    this.trans = random(70, 110);
    this.hu = random(120, 200);
    this.lifespanDecay = random(0.5, 1.5);
  }
  update(bass, mid, treble, level) {
    super.update(bass, mid, treble, level);
    if (mid > 180) {
      this.speedMult = map(mid, 180, 255, 1, 1.5);
    } else {
      this.speedMult = lerp(this.speedMult, 1, 0.05);
    }
  }
  display() {
    let alpha = useLifespan ? this.lifespan : this.trans;
    let distance = dist(this.position0.x, this.position0.y, 
                        this.position.x, this.position.y);
    if (distance < 50) {
      if (bw) {
        stroke(255, alpha);
      } else {
        stroke(this.hu, 150, 255, alpha);
      }
      strokeWeight(this.strokeWeightVal);
      line(this.position0.x, this.position0.y, this.position.x, this.position.y);
    }
  }
}
```

</details>

<details>
    <summary>mobile_treble.js</summary><br>

```js
// TREBLE(Agudos)
class TrebleMobile extends Mobile {
  constructor(index) {
    super(index);
    this.mass = 0.5;
    this.speedMult = 1.5;
    this.strokeWeightVal = 0.3;
    this.trans = random(50, 90);
    this.hu = random(280, 360);
    this.lifespanDecay = random(1, 2.5);
  }
  update(bass, mid, treble, level) {
    super.update(bass, mid, treble, level);
    if (treble > 160) {
      this.speedMult = map(treble, 160, 255, 1.5, 2.5);
      this.trans = constrain(this.trans + 3, 50, 150);
    } else {
      this.speedMult = lerp(this.speedMult, 1.5, 0.05);
      this.trans = lerp(this.trans, 70, 0.05);
    }
  }
  display() {
    let alpha = useLifespan ? this.lifespan : this.trans;
    let distance = dist(this.position0.x, this.position0.y, 
                       this.position.x, this.position.y);
    if (distance < 50) {
      if (bw) {
        stroke(255, alpha);
      } else {
        stroke(this.hu, 255, 255, alpha);
      }
      strokeWeight(this.strokeWeightVal);
      line(this.position0.x, this.position0.y, this.position.x, this.position.y);
    }
  }
}
```

</details>


</details>


---

<img width="1000" src="https://github.com/user-attachments/assets/c34d2466-31c2-474f-8966-dfacd0367669">


### 📸 FASE 3: CAPTURAS

#### Capturas de Pantalla


**🔗 Enlace p5.js:** [Ver en vivo](https://editor.p5js.org/DanieLudens/sketches/uMtu_LL9O)

**🎵 Canción por defecto:** [Enjoy the silence - Depeche Mode](https://open.spotify.com/intl-es/track/6WK9dVrRABMkUXFLNlgWFh?si=3c5ab583d13d48cb)

|Muestra|Descripción|
|:---:|:---:|
|<img width="500" src="https://github.com/user-attachments/assets/7e252b90-7a29-4cbc-a24d-88dce8fc8a67"> | Visualizacion completo con UI|
|<img width="500" src="https://github.com/user-attachments/assets/912850a7-470f-4ea4-abf1-6bd145b98340"> | Toggle UI con botón o tecla 'U'|
|<img width="500" src="https://github.com/user-attachments/assets/cab9ee42-78ba-4804-a217-f4c82e273f57"> | Intrucciones|
|<img width="500" src="https://github.com/user-attachments/assets/0d979552-1a3e-4b2e-87ff-4c30bf62ce54"> | Información|
|<img width="500" src="https://github.com/user-attachments/assets/edaa8168-e686-4fc4-b445-d60e162fb1d2"> | Barras de frecuencia |
|<img width="500" src="https://github.com/user-attachments/assets/beaf9d48-498e-4d12-854b-856dca3677b6"> | Volumen = 0% = Solo Flow Field Pasivo|
|<img width="500" src="https://github.com/user-attachments/assets/ba92051e-e1df-4185-99b4-a6ad870dd549"> | Volumen > 0% = Reactivo|
|<img width="500" src="https://github.com/user-attachments/assets/c46b2ad8-460f-4ae2-a836-12b075a50cd3"> | Barra espaciadora o 'R' Reset = diferentes patrones|
|<img width="500" src="https://github.com/user-attachments/assets/ba92a3f5-9ecd-4edc-a380-055acbf0ea90"> | una muestra de un patrón|






---


## Reflect: Consolidación y metacognición 🤔

Una vez termines esta unidad invierte en ti unos minutos para reflexionar sobre tu proceso de aprendizaje.

1. Realiza un diagrama conceptual donde incluyas todos los conceptos que has aprendido en las unidades 1 a 5.



2. ¿Qué has venido haciendo bien en tu proceso durante el curso que debas mantener en la próxima unidad?



3. ¿Qué has venido haciendo mal en tu proceso durante el curso que debas cambiar en la próxima unidad?


# Unidad 3


## 🛠 Fase: Apply

En esta fase, aplicaré los conceptos aprendidos creando una obra generativa interactiva en tiempo real que involucren diferentes tipos de fuerzas. Inspirada en el problema de los n-cuerpos crearé algo diferente basado en ese concepto y en las esculturas cinéticas de Alexander Calder.

Basada en la exploración que hice en la [Unidad 4 Actividad 7: repaso de conceptos de unidades anteriores: Ramitas](https://github.com/jfUPB/simulacion-2025-20-DannieLudens/blob/unidad4/reflect/unidad-4/set-seek.md)

  <img width="400" src="https://github.com/user-attachments/assets/624cfd85-98c0-4b34-a68f-ea529ecc5a20">

---

### Actividad 10: Problema de los N-Cuerpos y Moviles de Calder

<img width="400" src="https://github.com/user-attachments/assets/b5151d31-45d3-4bc4-91d9-771af1fa8c79"> Viento moderado y baja gravedad


<details>
  <summary>Más Muestras (Click Aquí)</summary>

<img width="400" src="https://github.com/user-attachments/assets/2bf59e8a-f824-4f36-a2a4-da36300b3360"> 2 minutos + interactividad con nodos

<img width="400" src="https://github.com/user-attachments/assets/38f8457f-55cf-4e13-9742-f87e33dd5b43"> Aumento de viento al máximo

<img width="400" src="https://github.com/user-attachments/assets/3ad7a3bb-4275-4623-b3a4-8c6495ed15b3"> Aumento de gravedad extrema

<img width="400" src="https://github.com/user-attachments/assets/0c48821c-c3cb-4db0-a881-dd7ade441781"> Limpieza de canvas para ver los trazos

</details>

<img width="600" src="https://github.com/user-attachments/assets/f9951881-a582-4271-9ed7-0b73df2e46a2"> Viento bajo y gravedad extrema

### Enlace al Editor de p5js

[Link al Sketch en p5js](https://editor.p5js.org/DanieLudens/sketches/yhk8vG-25)

### Codigo

<details>
  <summary>sketch.js</summary><br>
                      
```js
// Inspirado en The Nature of Code Daniel Shiffman - Moviles de Calder 
// Sistema de balancines equilibrados afectados por viento y fuerzas gravitacionales mutuas

let mobile;
let windStrength = 2.0; 
let trailLayer; // Capa separada para los trazos de las figuras
let allMasses = []; 

// Constantes para el problema de n-cuerpos
let gravitationalConstant = 0.5; // Intensidad atracción gravitacional
let nBodyEnabled = true; // Activar/desactivar fuerzas gravitacionales

// Paleta de colores de Calder - Solo colores primarios
let calderColors = [
  [220, 20, 20],   // Rojo Calder
  [255, 220, 0],   // Amarillo Calder  
  [30, 70, 200]    // Azul Calder
];

function setup() {
  createCanvas(800, 700);
  
  // Crear capa separada para los trails
  trailLayer = createGraphics(width, height);
  trailLayer.background(220, 220, 220); // Fondo gris claro
  
  // Crear móvil de Calder con estructura de balancines
  // Nivel 1: Balancín principal
  mobile = new BalanceArm(createVector(width / 2, 150), 100);
  mobile.horizontalLength = 180;
  
  // Lado izquierdo: balancín con formas características
  let leftArm = mobile.addLeftArm(80, 120);
  let mass1 = leftArm.addLeftMass(60, 20, 'circle');    
  let mass2 = leftArm.addRightMass(70, 18, 'triangle'); 
  
  // Lado derecho: estructura más compleja
  let rightArm = mobile.addRightArm(90, 140);
  
  // Del balancín derecho: un sub-balancín
  let rightLeftArm = rightArm.addLeftArm(60, 100);
  let mass3 = rightLeftArm.addLeftMass(50, 16, 'square');  
  let mass4 = rightLeftArm.addRightMass(55, 22, 'circle'); 
  
  // Elemento terminal del lado derecho
  let mass5 = rightArm.addRightMass(75, 25, 'triangle');   
  
  // Recopilar todas las masas
  allMasses = [mass1, mass2, mass3, mass4, mass5];
  
  // Asignar colores garantizando que aparezcan los 3 primarios
  assignGuaranteedColors();
}

function draw() {
  // Fondo gris claro para resaltar colores de Calder
  background(220, 220, 220);
  
  // Mostrar la capa de trails primero
  image(trailLayer, 0, 0);
  
  // Dibujar punto de anclaje en el techo
  stroke(0);
  strokeWeight(8);
  point(mobile.origin.x, mobile.origin.y);
  
  // Actualizar y mostrar el móvil completo
  mobile.update();
  mobile.show();
  
  // Instrucciones
  fill(0);
  noStroke();
  textSize(14);
  text("Arrastra los Nodos grises o las figuras para moverlas", 10, 20);
  text("Móvil de Calder x Problema de N-Cuerpos", 10, 40);
  text("Flechas ↑↓ viento: " + windStrength.toFixed(1) + " | G gravedad: " + gravitationalConstant.toFixed(1), 10, 60);
  text("ESPACIO: limpiar | G: gravedad ON/OFF | +/-: ajustar gravedad", 10, 80);
  text("N-Cuerpos: " + (nBodyEnabled ? "ACTIVADO" : "DESACTIVADO"), 10, 100);
  
  // Indicador visual del viento
  drawWindIndicator();
}

function drawWindIndicator() {
  // Dibujar indicador de viento en la esquina
  push();
  translate(width - 100, 30);
  
  // Fondo
  fill(255, 255, 255, 200);
  stroke(0);
  strokeWeight(1);
  rect(0, 0, 80, 40, 5);
  
  // Texto
  fill(0);
  noStroke();
  textSize(10);
  textAlign(CENTER);
  text("VIENTO", 40, 12);
  
  // Barra de intensidad
  let barWidth = map(windStrength, 0, 5, 0, 60);
  fill(100, 150, 255);
  rect(10, 18, barWidth, 10);
  stroke(0);
  noFill();
  rect(10, 18, 60, 10);
  
  pop();
}

function keyPressed() {
  if (keyCode === UP_ARROW) {
    windStrength = min(windStrength + 0.5, 5.0);
  } else if (keyCode === DOWN_ARROW) {
    windStrength = max(windStrength - 0.5, 0.0);
  } else if (key === ' ') {
    // Limpiar todos los trails
    clearAllTrails(mobile);
    trailLayer.background(220, 220, 220); // Limpiar capa de trails con gris claro
  } else if (key === 'g' || key === 'G') {
    // Activar/desactivar fuerzas gravitacionales
    nBodyEnabled = !nBodyEnabled;
  } else if (key === '+' || key === '=') {
    // Aumentar constante gravitacional en saltos de 5 (para experimentos extremos)
    gravitationalConstant += 5.0;
  } else if (key === '-') {
    // Disminuir constante gravitacional en saltos de 5
    gravitationalConstant = max(gravitationalConstant - 5.0, 0.0);
  }
}

function assignGuaranteedColors() {
  // Garantizar que al menos aparezca un color de cada tipo
  let colorAssignments = [];
  
  // Asignar obligatoriamente los 3 colores primarios a las primeras 3 masas
  for (let i = 0; i < min(3, allMasses.length); i++) {
    colorAssignments.push(i);
  }
  
  // Para las masas restantes, asignar colores aleatorios
  for (let i = 3; i < allMasses.length; i++) {
    colorAssignments.push(floor(random(calderColors.length)));
  }
  
  // Mezclar las asignaciones para que no siempre siga el mismo orden
  colorAssignments = shuffle(colorAssignments);
  
  // Asignar colores a las masas
  for (let i = 0; i < allMasses.length; i++) {
    let colorIndex = colorAssignments[i];
    allMasses[i].color = color(
      calderColors[colorIndex][0], 
      calderColors[colorIndex][1], 
      calderColors[colorIndex][2]
    );
  }
}

function clearAllTrails(element) {
  // Función recursiva para limpiar trails de todas las masas
  if (element instanceof Mass) {
    element.trail = [];
  } else if (element instanceof BalanceArm) {
    if (element.leftElement) clearAllTrails(element.leftElement);
    if (element.rightElement) clearAllTrails(element.rightElement);
  }
}

function mousePressed() {
  mobile.startDrag();
}

function mouseReleased() {
  mobile.stopDrag();
}
```

</details>

<details>
  <summary>BalanceArm.js</summary><br>
                      
```js

class BalanceArm {
  constructor(origin, armLength) {
    // Punto de suspensión del balancín
    this.origin = origin.copy();
    
    // Longitud del brazo vertical (cuerda de suspensión)
    this.suspensionLength = armLength;
    
    // Ángulo del balancín (rotación)
    this.angle = random(-PI/6, PI/6);
    this.angleVelocity = 0;
    this.angleAcceleration = 0;
    
    // Posición del centro del balancín
    this.center = createVector();
    
    // Longitud del brazo horizontal
    this.horizontalLength = 100;
    
    // Elementos colgando de los extremos del balancín
    this.leftElement = null;  // Puede ser una masa o otro balancín
    this.rightElement = null; // Puede ser una masa o otro balancín
    
    // Física
    this.gravity = 0.4;
    this.damping = 0.995;
    
    // Arrastre
    this.dragging = false;
    
    // Offset único para Perlin noise
    this.noiseOffsetX = random(1000);
    this.noiseOffsetY = random(1000, 2000);
  }
  
  // Agregar una masa en el lado izquierdo
  addLeftMass(hangLength, mass, shapeType) {
    this.leftElement = new Mass(this.getLeftPoint(), hangLength, mass, shapeType);
    return this.leftElement;
  }
  
  // Agregar una masa en el lado derecho
  addRightMass(hangLength, mass, shapeType) {
    this.rightElement = new Mass(this.getLeftPoint(), hangLength, mass, shapeType);
    return this.rightElement;
  }
  
  // Agregar un balancín en el lado izquierdo
  addLeftArm(hangLength, armLength) {
    this.leftElement = new BalanceArm(this.getLeftPoint(), hangLength);
    this.leftElement.horizontalLength = armLength;
    return this.leftElement;
  }
  
  // Agregar un balancín en el lado derecho
  addRightArm(hangLength, armLength) {
    this.rightElement = new BalanceArm(this.getRightPoint(), hangLength);
    this.rightElement.horizontalLength = armLength;
    return this.rightElement;
  }
  
  // Obtener el punto izquierdo del balancín
  getLeftPoint() {
    return createVector(
      this.center.x - this.horizontalLength * cos(this.angle),
      this.center.y - this.horizontalLength * sin(this.angle)
    );
  }
  
  // Obtener el punto derecho del balancín
  getRightPoint() {
    return createVector(
      this.center.x + this.horizontalLength * cos(this.angle),
      this.center.y + this.horizontalLength * sin(this.angle)
    );
  }
  
  applyWind(windForce) {
    // El viento afecta el ángulo del péndulo del balancín
    if (!this.dragging) {
      // Convertir fuerza de viento en influencia angular
      let windInfluence = windForce.x * 0.001;
      this.angleVelocity += windInfluence;
    }
  }
  
  update() {
    // Si estamos arrastrando, calcular ángulo basado en el mouse
    if (this.dragging) {
      let diff = p5.Vector.sub(createVector(mouseX, mouseY), this.origin);
      let pendulumAngle = atan2(diff.x, diff.y);
      this.angleVelocity = 0;
      
      // Actualizar posición del centro
      this.center.x = this.origin.x + this.suspensionLength * sin(pendulumAngle);
      this.center.y = this.origin.y + this.suspensionLength * cos(pendulumAngle);
    } else {
      // Aplicar viento usando Perlin noise
      let windX = map(noise(this.noiseOffsetX), 0, 1, -windStrength, windStrength);
      let windY = map(noise(this.noiseOffsetY), 0, 1, -windStrength * 0.3, windStrength * 0.3);
      let windForce = createVector(windX, windY);
      this.applyWind(windForce);
      this.noiseOffsetX += 0.005;
      this.noiseOffsetY += 0.005;
      
      // Física del péndulo para el balancín
      let pendulumAngle = atan2(this.center.x - this.origin.x, this.center.y - this.origin.y);
      let angleAccel = (-this.gravity / this.suspensionLength) * sin(pendulumAngle);
      this.angleVelocity += angleAccel;
      this.angleVelocity *= this.damping;
      pendulumAngle += this.angleVelocity;
      
      // Actualizar posición del centro
      this.center.x = this.origin.x + this.suspensionLength * sin(pendulumAngle);
      this.center.y = this.origin.y + this.suspensionLength * cos(pendulumAngle);
      
      // Rotación del balancín basada en el balance de masas
      // (Simplificado - en realidad depende de los torques)
      this.angleAcceleration = 0;
      this.angleVelocity += this.angleAcceleration;
      this.angleVelocity *= this.damping;
      this.angle += this.angleVelocity;
    }
    
    // Actualizar elementos izquierdo y derecho
    if (this.leftElement) {
      this.leftElement.origin = this.getLeftPoint();
      this.leftElement.update();
    }
    if (this.rightElement) {
      this.rightElement.origin = this.getRightPoint();
      this.rightElement.update();
    }
  }
  
  show() {
    // Cuerda de suspensión
    stroke(0);
    strokeWeight(2);
    line(this.origin.x, this.origin.y, this.center.x, this.center.y);
    
    // Balancín horizontal
    stroke(0);
    strokeWeight(3);
    let leftPt = this.getLeftPoint();
    let rightPt = this.getRightPoint();
    line(leftPt.x, leftPt.y, rightPt.x, rightPt.y);
    
    // Punto central del balancín
    fill(this.dragging ? color(255, 200, 0) : color(100));
    stroke(0);
    strokeWeight(2);
    ellipse(this.center.x, this.center.y, 8, 8);
    
    // Mostrar elementos
    if (this.leftElement) this.leftElement.show();
    if (this.rightElement) this.rightElement.show();
  }
  
  isMouseOver() {
    let d = dist(mouseX, mouseY, this.center.x, this.center.y);
    return d < 15;
  }
  
  startDrag() {
    if (this.isMouseOver()) {
      this.dragging = true;
      return true;
    }
    
    // Intentar con elementos hijos
    if (this.leftElement && this.leftElement.startDrag()) return true;
    if (this.rightElement && this.rightElement.startDrag()) return true;
    
    return false;
  }
  
  stopDrag() {
    this.dragging = false;
    if (this.leftElement) this.leftElement.stopDrag();
    if (this.rightElement) this.rightElement.stopDrag();
  }
}
```

</details>

<details>
  <summary>Mobile.js</summary><br>
                      
```js
// Clase para las masas (figuras colgantes)
class Mass {
  constructor(origin, hangLength, mass, shapeType) {
    this.origin = origin.copy();
    this.hangLength = hangLength;
    
    // Ángulo del péndulo
    this.angle = random(-PI/6, PI/6);
    this.angleVelocity = 0;
    
    // Posición
    this.position = createVector();
    
    // Propiedades
    this.mass = mass || 20;
    this.shapeType = shapeType || 'circle';
    
    // Color será asignado por el sistema de colores garantizado
    this.color = null; // Se asignará después
    
    // Física
    this.gravity = 0.4;
    this.damping = 0.995;
    
    // Arrastre
    this.dragging = false;
    
    // Offset único para Perlin noise
    this.noiseOffsetX = random(2000, 3000);
    this.noiseOffsetY = random(3000, 4000);
    
    // Trail (rastro)
    this.trail = [];
    this.maxTrailLength = 100; // Número de puntos en el rastro
  }
  
  applyWind(windForce) {
    // El viento afecta más a las masas más ligeras
    if (!this.dragging) {
      let windInfluence = windForce.x * (1 / this.mass) * 0.05;
      this.angleVelocity += windInfluence;
    }
  }
  
  // Calcular fuerza gravitacional hacia otra masa (Ley de Newton)
  calculateGravitationalForce(otherMass) {
    // Vector de distancia entre las dos masas
    let force = p5.Vector.sub(otherMass.position, this.position);
    let distance = force.mag();
    
    // Evitar división por cero y fuerzas extremas
    distance = constrain(distance, 20, 200);
    
    // F = G * m1 * m2 / r²
    let strength = (gravitationalConstant * this.mass * otherMass.mass) / (distance * distance);
    
    // Normalizar y aplicar magnitud
    force.normalize();
    force.mult(strength);
    
    return force;
  }
  
  // Aplicar fuerzas gravitacionales de todas las demás masas (N-Cuerpos)
  applyNBodyForces(masses) {
    if (!nBodyEnabled || this.dragging) return;
    
    for (let otherMass of masses) {
      if (otherMass !== this) { // No aplicar fuerza a sí misma
        let gravitationalForce = this.calculateGravitationalForce(otherMass);
        
        // Convertir fuerza cartesiana en influencia angular
        // La componente X de la fuerza afecta el ángulo del péndulo
        let angularInfluence = gravitationalForce.x * 0.001;
        this.angleVelocity += angularInfluence;
      }
    }
  }
  
  update() {
    if (this.dragging) {
      let diff = p5.Vector.sub(createVector(mouseX, mouseY), this.origin);
      this.angle = atan2(diff.x, diff.y);
      this.angleVelocity = 0;
    } else {
      // Aplicar viento usando Perlin noise
      let windX = map(noise(this.noiseOffsetX), 0, 1, -windStrength, windStrength);
      let windY = map(noise(this.noiseOffsetY), 0, 1, -windStrength * 0.3, windStrength * 0.3);
      let windForce = createVector(windX, windY);
      this.applyWind(windForce);
      this.noiseOffsetX += 0.008;
      this.noiseOffsetY += 0.008;
      
      // *** PROBLEMA DE N-CUERPOS ***
      // Aplicar fuerzas gravitacionales de todas las demás masas
      this.applyNBodyForces(allMasses);
      
      let angleAccel = (-this.gravity / this.hangLength) * sin(this.angle);
      this.angleVelocity += angleAccel;
      this.angleVelocity *= this.damping;
      this.angle += this.angleVelocity;
    }
    
    this.position.x = this.origin.x + this.hangLength * sin(this.angle);
    this.position.y = this.origin.y + this.hangLength * cos(this.angle);
    
    // Agregar posición actual al trail
    this.trail.push(this.position.copy());
    
    // Dibujar el nuevo segmento del trail inmediatamente con tonos más claros
    if (this.trail.length >= 2) {
      let lastIndex = this.trail.length - 1;
      
      // Crear versión más clara del color (mezclar con blanco)
      let lightR = lerp(red(this.color), 255, 0.6);   // 60% hacia el blanco
      let lightG = lerp(green(this.color), 255, 0.6);
      let lightB = lerp(blue(this.color), 255, 0.6);
      
      let alpha = 120; // Opacidad más suave
      trailLayer.stroke(lightR, lightG, lightB, alpha);
      trailLayer.strokeWeight(2); // Grosor más delgado
      trailLayer.line(
        this.trail[lastIndex-1].x, this.trail[lastIndex-1].y,
        this.trail[lastIndex].x, this.trail[lastIndex].y
      );
    }
    
    // Limitar longitud del trail
    if (this.trail.length > this.maxTrailLength) {
      this.trail.shift(); // Eliminar el punto más antiguo
    }
  }
  
  show() {
    // Cuerda
    stroke(0);
    strokeWeight(1);
    line(this.origin.x, this.origin.y, this.position.x, this.position.y);
    
    // Masa
    fill(this.dragging ? color(255, 200, 0) : this.color);
    stroke(0);
    strokeWeight(2);
    
    push();
    translate(this.position.x, this.position.y);
    
    if (this.shapeType === 'circle') {
      ellipse(0, 0, this.mass * 2, this.mass * 2);
    } else if (this.shapeType === 'square') {
      rectMode(CENTER);
      rect(0, 0, this.mass * 2, this.mass * 2);
    } else if (this.shapeType === 'triangle') {
      let size = this.mass * 1.5;
      triangle(0, -size, -size, size, size, size);
    }
    
    pop();
  }
  
  isMouseOver() {
    let d = dist(mouseX, mouseY, this.position.x, this.position.y);
    return d < this.mass;
  }
  
  startDrag() {
    if (this.isMouseOver()) {
      this.dragging = true;
      return true;
    }
    return false;
  }
  
  stopDrag() {
    this.dragging = false;
  }
  
  drawTrail() {
    if (this.trail.length < 2) return;
    
    // Dibujar en la capa separada de trails
    trailLayer.noFill();
    
    // Dibujar líneas conectando los puntos del trail
    for (let i = 1; i < this.trail.length; i++) {
      // Crear versión más clara del color
      let lightR = lerp(red(this.color), 255, 0.6);
      let lightG = lerp(green(this.color), 255, 0.6);
      let lightB = lerp(blue(this.color), 255, 0.6);
      
      // Transparencia gradual más sutil
      let alpha = map(i, 0, this.trail.length, 30, 120);
      trailLayer.stroke(lightR, lightG, lightB, alpha);
      
      // Grosor del trail más delgado
      let weight = map(i, 0, this.trail.length, 0.5, 2);
      trailLayer.strokeWeight(weight);
      
      // Dibujar línea desde el punto anterior al actual
      trailLayer.line(
        this.trail[i-1].x, this.trail[i-1].y,
        this.trail[i].x, this.trail[i].y
      );
    }
  }
}
```

</details>

### **Descripción de la Obra**

#### **Concepto**
Esta obra combina la estética base del equilibrio de los móviles de Alexander Calder con la complejidad física del problema de n-cuerpos. El resultado es un sistema donde 5 masas colgantes interactúan gravitacionalmente entre sí, creando patrones de movimiento únicos e impredecibles.

### **Interactividad**

#### **Controles del Usuario:**
- **Drag & Drop**: Arrastra cualquier figura o balancín para moverlo
- **Flechas ↑↓**: Ajustar intensidad del viento (0.0 - 5.0)
- **G**: Activar/Desactivar fuerzas gravitacionales
- **+ / -**: Aumentar/Disminuir constante gravitacional (saltos de 5.0)
- **ESPACIO**: Limpiar todos los trails del canvas

#### **Feedback Visual:**
- **Trails permanentes**: Cada masa deja un rastro de su trayectoria en tonos claros
- **Indicador de viento**: Barra visual que muestra la intensidad actual
- **Información en tiempo real**: Estado de todos los parámetros físicos
- **Color de arrastre**: Las figuras se vuelven amarillas al ser arrastradas

---

### **Randomness Utilizados**

#### **Perlin Noise (`noise()`)**
```javascript
// Aplicación: Viento orgánico y natural
let windX = map(noise(this.noiseOffsetX), 0, 1, -windStrength, windStrength);
let windY = map(noise(this.noiseOffsetY), 0, 1, -windStrength * 0.3, windStrength * 0.3);
```
- **Propósito**: Genera viento que nunca se repite, creando movimiento fluido y orgánico
- **Características**: Cada masa tiene offsets únicos para variedad individual

#### **Random (`random()`)**
```javascript
// Aplicaciones múltiples:
this.angle = random(-PI/6, PI/6);           // Ángulos iniciales variados
this.noiseOffsetX = random(2000, 3000);     // Offsets únicos en Perlin space
let colorIndex = floor(random(calderColors.length)); // Selección de colores
```
- **Propósito**: Diversidad en condiciones iniciales y comportamientos únicos

#### **Shuffle (`shuffle()`)**
```javascript
// Garantizar diversidad cromática:
colorAssignments = shuffle(colorAssignments);
```
- **Propósito**: Mezcla la asignación de colores garantizando que siempre aparezcan los 3 colores primarios de Calder en posiciones variables

---

### **Modelado del Problema de N-Cuerpos**

#### **Ley de Gravitación Universal**
Cada masa calcula la fuerza gravitacional hacia todas las demás masas usando:

```javascript
// F = G * m1 * m2 / r²
let strength = (gravitationalConstant * this.mass * otherMass.mass) / (distance * distance);
```

#### **Sistema de Interacciones**
- **5 masas** interactúan simultáneamente
- **20 fuerzas gravitacionales** calculadas en cada frame (cada masa hacia las otras 4)
- **Suma vectorial** de todas las fuerzas sobre cada masa

#### **Proceso por Frame**
1. **Para cada masa**: Calcula fuerza hacia todas las demás masas individualmente
2. **Suma fuerzas**: Cada masa experimenta la suma vectorial de 4 fuerzas gravitacionales
3. **Conversión angular**: Las fuerzas cartesianas se convierten en influencias sobre la velocidad angular del péndulo
4. **Aplicación**: `angularInfluence = gravitationalForce.x * 0.001`

#### **Características del N-Cuerpos**
- **No linealidad**: Pequeños cambios en posición generan grandes diferencias en comportamiento
- **Impredecibilidad**: Los patrones nunca se repiten exactamente debido a la sensibilidad a condiciones iniciales
- **Comportamiento emergente**: El movimiento de cada masa afecta a todas las demás simultáneamente
- **Escalabilidad**: La constante gravitacional es ajustable permitiendo desde movimientos sutiles hasta caos gravitacional


### **Características del codigoo**

#### **Arquitectura del Sistema**
- **Clase BalanceArm**: Representa balancines con brazos horizontales
- **Clase Mass**: Representa las figuras finales (círculos, cuadrados, triángulos)
- **Sistema jerárquico**: Cada balancín puede contener otros balancines o masas
- **Física híbrida**: Combina péndulos simples con fuerzas gravitacionales

#### **Renderizado Avanzado**
- **Doble capa**: Canvas principal + capa separada para trails permanentes (trazos de dibujos)
- **Trazos de contraste**: Tonos más claros que no compiten visualmente con las figuras
- **Colores garantizados**: Sistema que asegura la presencia de los 3 colores primarios de Calder

#### **Optimización de Performance**
- **Constraints de distancia**: Evita cálculos extremos y divisiones por cero
- **Límites de fuerza**: Previene comportamientos inestables
- **Renderizado eficiente**: Los trails se dibujan una sola vez y persisten

#### **De las unidades anteriores de "The Nature of Code"**
- **Unidad 0 (Randomness)**: Perlin noise, random, shuffle
- **Unidad 2 (Forces)**: Gravitación, viento, fricción
- **Unidad 3 (Oscillation)**: Péndulos, movimiento angular

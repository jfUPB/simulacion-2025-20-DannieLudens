# Unidad 6

## Introducción 📜

> [!NOTE]
> **Agentes autónomos y comportamiento emergente**
>
> Continuando nuestro viaje por “The Nature of Code”, nos adentramos en el fascinante mundo de los **agentes autónomos**. Estos son entidades que operan independientemente en un entorno, tomando decisiones basadas en un conjunto de reglas y en la percepción de su entorno (incluyendo otros agentes).
>
> En esta unidad, exploraremos cómo algunas reglas simples a nivel individual pueden dar lugar a comportamientos colectivos complejos y a menudo inesperados, un fenómeno conocido como emergencia. Nos centraremos en dos algoritmos clave presentados en “The Nature of Code”: los **campos de flujo (flow fields)** y el **comportamiento de enjambre (flocking).**

## Set: ¿Qué aprenderás en esta unidad? 💡

> [!NOTE]
> **Objetivos de aprendizaje**
>
> Al finalizar esta unidad, serás capaz de:
>
> - Describir el concepto de agente autónomo y comportamiento emergente en el contexto del arte generativo.
> - Analizar y explicar el funcionamiento de los algoritmos de campos de flujo (flow fields) y comportamiento de enjambre (flocking) presentados en “The Nature of Code”.
> - Identificar los parámetros clave que controlan el comportamiento de los agentes en ambos algoritmos.
> - Implementar un sketch interactivo en p5.js que utilice flow fields o flocking.
> - Aplicar creativamente uno de estos algoritmos.


### Actividad 01

En esta actividad propondré que te encuentres de nuevo el trabajo de [Tyler Hobbs](https://youtu.be/8tTGJvijoDw?si=7KWuEhMTIjH41JOj) y específicamente que mires [su artículo sobre campos de flujo.](https://www.tylerxhobbs.com/words/flow-fields)

> [!NOTE]
> 🧐🧪✍️ Reporta en tu bitácora
>
> - Captura en tu bitácora dos imágenes de Tyler Hobbs que te llamen la atención y explica por qué.
> - ¿Qué te inspira de su trabajo?


|Obra|Imagen|Que me inspira y ¿por qué llama mi atención?|
|:---:|---|:---|
|[Ectogenesis](https://www.tylerxhobbs.com/works/ectogenesis#info) 2019|<img width="300" src="https://github.com/user-attachments/assets/c919aa66-19cf-427f-86b2-f63d7b42807d" />|Me llama la atención que mezcla elementos para formar flow fields, ya no solo como una sola particula sino una serie de reglas para construir un organismo y que se muevan fluidos por el cuadro, aunque es estática genera una dinamismo al observarla y me imaginaria como fluiria si tuviera vida.|
|[Catcher](https://www.tylerxhobbs.com/works/catcher#info) 2016|<img width="300" src="https://github.com/user-attachments/assets/cb8de7a7-a6ba-4f6f-95f8-70248980c58c" />|En Catcher refleja el dinamismo de la naturaleza y el viento afectando las plantas pero la obra en si es una representación de la naturaleza hasta que se acerca al lienzo y se nos damos cuenta que son lineas y puntos, me llama mucho la atención el comportamiento de las particulas ordenadas y armónicas|

El trabajo de Tyler Hobbs me inspira por:

1. **Simplicidad que genera complejidad**: Usa elementos básicos (puntos, líneas, curvas) pero crea obras visualmente ricas
2. **Ilusión de vida**: Sus obras estáticas logran transmitir movimiento y dinamismo
3. **Naturaleza algorítmica revelada**: No oculta que es arte generativo, celebra la belleza de los algoritmos
4. **Balance entre control y caos**: Hay estructura, pero también variación orgánica

## Seek: Investigación 🔎

> [!NOTE]
> Analizando los algoritmos
> 
> Vamos a sumergirnos en el código y la lógica detrás de los campos de flujo y el comportamiento de enjambre, utilizando los ejemplos de “The Nature of Code” como punto de partida. El objetivo es entender cómo funcionan internamente.

### Actividad 02

En esta actividad quiero que investigues alrededor de estas tres preguntas:

<details>
<summary>1. ¿Qué es una fuerza de dirección (steering force)?</summary><br>

Una **steering force** (fuerza de dirección) es una fuerza que le indica al agente "hacia dónde debería dirigirse" para alcanzar un comportamiento deseado. No es simplemente mover el agente a un punto, sino calcular la fuerza necesaria para que el agente SE DIRIJA hacia ese comportamiento de manera gradual y natural.

```
steering = velocidad_deseada - velocidad_actual
```

**Componentes**

- **Velocidad deseada**: Hacia dónde QUEREMOS que vaya el agente (dirección + velocidad ideal)
- **Velocidad actual**: Hacia dónde va AHORA el agente
- **Steering force**: La diferencia entre ambas - qué fuerza aplicar para "corregir el rumbo"

**Características importantes**

1. **Limitación de fuerza**: Se limita con un `maxforce` - el agente no puede girar instantáneamente, lo que genera movimientos más realistas
2. **Movimientos naturales**: Produce trayectorias suaves y orgánicas, no saltos bruscos
3. **Combinabilidad**: Permite comportamientos complejos combinando múltiples steering forces
4. **Autonomía**: El agente "decide" su movimiento basándose en lo que percibe

**Ejemplo visual**
```
Agente actual: →
Objetivo:      ↗
Steering:      ↑ (la fuerza que lo "jala" hacia arriba para corregir rumbo)
```

</details>


<details>
<summary>2. ¿Qué diferencia tiene con las fuerzas tradicionales?</summary><br>

**Fuerzas físicas tradicionales (que ya hemos estudiado)**

**Ejemplos**:
- **Gravedad**: Siempre apunta hacia abajo, es constante
- **Fricción**: Opuesta al movimiento, proporcional a la velocidad
- **Viento**: Empuja en una dirección fija o variable
- **Empuje**: Fuerza externa aplicada al objeto

**Características**:
- Son fuerzas **externas** al agente
- Son fuerzas **del entorno** o del mundo físico
- Son **pasivas** - el objeto las "sufre"
- Son **predecibles** - siguen leyes físicas fijas

**Steering forces (nuevas)**

**Características**:
- Son fuerzas **internas** - el agente las calcula basándose en su "deseo"
- Son **dinámicas e inteligentes** - cambian según lo que el agente "quiere hacer"
- **Dependen del estado actual** del agente (su velocidad y posición)
- Simulan **autonomía y toma de decisiones**
- Son **activas** - el agente "elige" aplicarlas

**Tabla comparativa**

| Aspecto | Fuerzas Tradicionales | Steering Forces |
|---------|----------------------|-----------------|
| **Origen** | Externas (entorno) | Internas (agente) |
| **Naturaleza** | Pasivas | Activas (autónomas) |
| **Comportamiento** | Fijas/predecibles | Dinámicas/adaptativas |
| **Objetivo** | Simular física | Simular inteligencia |
| **Ejemplo** | "La gravedad me jala" | "YO quiero ir allá" |

**Ejemplo comparativo**

**Escenario**: Un pájaro volando hacia un árbol

- **Con fuerzas tradicionales**:
  - Gravedad lo jala hacia abajo
  - Viento lo empuja lateralmente
  - El pájaro "sufre" estas fuerzas

- **Con steering forces**:
  - El pájaro "ve" el árbol
  - "Decide" dirigirse hacia él
  - Calcula la fuerza necesaria para llegar
  - Ajusta su trayectoria continuamente

**La diferencia clave**

Las steering forces permiten que el agente tenga **intención** y **autonomía**, mientras que las fuerzas tradicionales son reacciones al entorno. Esta es la base para crear agentes que parezcan "vivos" e "inteligentes".

</details>

<details>
<summary>3. Relación con Craig Reynolds</summary>

**¿Quién es Craig Reynolds?**

**Craig Reynolds** es un científico computacional e investigador pionero en el campo de:
- Simulación de comportamiento animal
- Gráficos por computadora
- Vida artificial
- Animación de personajes autónomos

**Sus contribuciones fundamentales**

**1. Boids (1986)**

- **Qué es**: Algoritmo que simula bandadas de pájaros (bird-oid droids → boids)
- **Innovación**: Demostró que comportamientos complejos de grupo emergen de **3 reglas simples**:
  - **Separación**: Evitar chocar con vecinos
  - **Alineación**: Ir en la misma dirección que vecinos
  - **Cohesión**: Mantenerse cerca del grupo
- **Impacto**: Primera demostración clara de comportamiento emergente en simulación

**2. Steering Behaviors for Autonomous Characters (1999)**

- **Qué es**: Formalización de un conjunto de comportamientos básicos de dirección
- **Comportamientos definidos**:
  - **Seek**: Ir hacia un objetivo
  - **Flee**: Huir de un peligro
  - **Arrival**: Llegar suavemente a un punto (desacelerando)
  - **Pursue**: Perseguir un objetivo móvil (predecir su posición)
  - **Evade**: Evadir una amenaza móvil
  - **Wander**: Vagar aleatoriamente
  - **Path following**: Seguir un camino
  - **Obstacle avoidance**: Evitar obstáculos
- **Legado**: Se convirtió en el estándar de la industria para IA de movimiento

**3. Aplicaciones en la industria**

**Cine y efectos visuales**:
- Batman Returns (1992): Simulación de murciélagos y pingüinos
- El Rey León: Simulación de estampidas de ñus
- Innumerables películas modernas usan sus algoritmos

**Videojuegos**:
- NPCs (personajes no jugables) con movimiento realista
- Simulación de multitudes
- Bandadas, manadas, cardúmenes

**Robótica**:
- Drones que vuelan en formación
- Robots autónomos que navegan evitando obstáculos
- Vehículos autónomos

**La conexión con Steering Forces**

**Por qué es importante**:

1. **Popularizó el concepto**: Reynolds demostró que agentes autónomos pueden tomar "decisiones" de movimiento que parecen inteligentes

2. **Formalizó la técnica**: Antes de él, no existía un framework claro para programar comportamientos autónomos

3. **Demostró la emergencia**: Con Boids, mostró que **autonomía + reglas simples = comportamiento emergente complejo**

4. **Steering forces son el mecanismo**: Las steering forces son la implementación técnica que permite realizar todos los comportamientos que Reynolds definió

**Cita clave de Reynolds**

> "The individual behaviors are simple, but the aggregate motion is complex and realistic."

(Los comportamientos individuales son simples, pero el movimiento agregado es complejo y realista)

**Recursos adicionales**

- [Sitio web de Craig Reynolds](http://www.red3d.com/cwr/)
- [Paper original de Boids (1987)](http://www.red3d.com/cwr/boids/)
- [Steering Behaviors (1999)](http://www.red3d.com/cwr/steer/)

</details>

<details>
<summary>Referencias consultadas</summary>

- The Nature of Code - Capítulo 5: Autonomous Agents
- Craig Reynolds - Steering Behaviors For Autonomous Characters
- Craig Reynolds - Boids Background and Update
- Tyler Hobbs - Flow Fields Article

</details>

> [!NOTE]
> 🧐🧪✍️ Reporta en tu bitácora
>
> Documenta tus hallazgos en tu bitácora.

### Actividad 03

> [!NOTE]
> 🎯 Enunciado
>
> Vamos a analizar el primer algoritmo clave: los campos de flujo (Flow Fields), basándonos en el ejemplo del libro “The Nature of Code”. Entenderemos cómo una cuadrícula de vectores dirige el movimiento de los agentes.

> [!TIP]
> Recursos
>
> - Libro “The Nature of Code” (TNoC) de Daniel Shiffman: [Capítulo 5, sección “Flow Fields”](https://natureofcode.com/autonomous-agents/#flow-fields) (y ejemplos de código asociados).
> - El código fuente del ejemplo de Flow Fields de TNoC.


#### Pasos:

1. **Ejecuta el ejemplo:** ejecuta el código del ejemplo principal de Flow Fields de TNoC en p5.js. Observa el comportamiento de los vehículos/agentes.
<details>
<summary>📋 Paso 1: Ejecutar el ejemplo</summary>

**Comportamiento observado**

- **120 vehículos/agentes** representados como triángulos que siguen el campo de flujo
- Los agentes se mueven de manera suave y orgánica siguiendo vectores invisibles
- El movimiento es fluido y continuo, sin saltos bruscos
- Los agentes atraviesan los bordes del canvas (wraparound)

**Controles interactivos**

- **Barra espaciadora**: Muestra/oculta las líneas de debug del campo de flujo (vectores visualizados como líneas)
- **Click del mouse**: Genera un nuevo campo de flujo aleatorio usando Perlin noise

**Observaciones**

Cuando activo el modo debug(barra espaciadora) , puedo ver la cuadrícula de vectores que forman el campo de flujo. Los agentes responden inmediatamente a estos vectores, creando patrones de movimiento complejos a partir de reglas simples.

</details>

2. **Identifica la estructura del campo:** en el código (usualmente en una clase `FlowField`), localiza cómo se almacena el campo de flujo. ¿Qué estructura de datos se usa (ej: un array 2D)? ¿Qué representa cada elemento de esa estructura? ¿Cómo se calcula inicialmente el vector en cada punto?


<details>
<summary>🏗️ Paso 2: Identificar la estructura del campo</summary>

**Estructura de datos utilizada**

**Tipo**: Array bidimensional (2D) de vectores `p5.Vector`

```javascript
this.field = new Array(this.cols);
for (let i = 0; i < this.cols; i++) {
  this.field[i] = new Array(this.rows);
}
```

**Dimensiones del campo**

El campo se divide en una cuadrícula basada en la **resolución**:

```javascript
this.cols = width / this.resolution;  // Número de columnas
this.rows = height / this.resolution;  // Número de filas
```

**Con los valores del ejemplo:**
- Canvas: 640 × 240 píxeles
- Resolución: 20 píxeles
- Columnas: 640 / 20 = **32 columnas**
- Filas: 240 / 20 = **12 filas**
- Total de vectores: 32 × 12 = **384 vectores**

**Qué representa cada elemento**

Cada elemento `field[i][j]` es un **vector de dirección** (`p5.Vector`) que indica:
- La dirección hacia la que deben moverse los agentes cuando pasan por esa celda
- Es un vector unitario (magnitud 1) que luego se escala según la velocidad del agente

**Cómo se calculan los vectores inicialmente**

**Método utilizado**: Perlin Noise

```javascript
init() {
  noiseSeed(random(10000));  // Semilla aleatoria para variación
  let xoff = 0;
  for (let i = 0; i < this.cols; i++) {
    let yoff = 0;
    for (let j = 0; j < this.rows; j++) {
      // Perlin noise genera valor entre 0 y 1
      let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
      // Convierte ángulo a vector unitario
      this.field[i][j] = p5.Vector.fromAngle(angle);
      yoff += 0.1;
    }
    xoff += 0.1;
  }
}
```

**Proceso paso a paso:**

1. **Perlin Noise** (`noise(xoff, yoff)`): Genera valores suaves y continuos entre 0 y 1
2. **Mapeo a ángulos**: Se mapea ese valor al rango [0, 2π] (0° a 360°)
3. **Conversión a vector**: `p5.Vector.fromAngle()` crea un vector unitario apuntando en esa dirección
4. **Incrementos pequeños** (0.1): Crean un campo suave donde vectores vecinos tienen direcciones similares

**Ventaja de Perlin Noise**:
- Genera campos de flujo orgánicos y naturales
- Transiciones suaves entre celdas adyacentes
- Evita cambios bruscos de dirección

**Visualización conceptual**

```
Campo de flujo (vista simplificada):
→ → ↗ ↑ ↑ ↖ ← ←
→ ↗ ↗ ↑ ↖ ↖ ← ↙
↘ → ↗ ↑ ↖ ← ↙ ↓
↓ ↘ → → ← ↙ ↓ ↓
```

Cada flecha representa un vector en una celda de la cuadrícula.

</details>

3. **Analiza el comportamiento del agente:** en el código de la clase del vehículo/agente (`Vehicle`), encuentra la función `follow()`. Explica con tus palabras:
  - ¿Cómo determina el agente qué vector del campo de flujo debe seguir basándose en su posición actual? (pista: implica mapear la posición a índices de la cuadrícula).
  - Una vez que tiene el vector deseado del campo, ¿Cómo lo utiliza para calcular la fuerza de dirección (`steering force`)? (pista: implica calcular la diferencia con la velocidad actual y limitar la fuerza).

<details>
<summary>🚗 Paso 3: Analizar el comportamiento del agente</summary>

**La función `follow()` del vehículo**

```javascript
follow(flow) {
  // 1. Obtener el vector deseado del campo
  let desired = flow.lookup(this.position);
  // 2. Escalar a la velocidad máxima
  desired.mult(this.maxspeed);
  // 3. Calcular steering force
  let steer = p5.Vector.sub(desired, this.velocity);
  // 4. Limitar la fuerza máxima
  steer.limit(this.maxforce);
  // 5. Aplicar la fuerza
  this.applyForce(steer);
}
```

<ins><strong>Parte A: ¿Cómo determina qué vector seguir?</strong></ins>

El agente usa la función `lookup()` del FlowField para mapear su posición al índice de la cuadrícula:

```javascript
lookup(position) {
  // Divide posición por resolución para obtener índices
  let column = constrain(floor(position.x / this.resolution), 0, this.cols - 1);
  let row = constrain(floor(position.y / this.resolution), 0, this.rows - 1);
  // Retorna copia del vector en esa celda
  return this.field[column][row].copy();
}
```

**Proceso de mapeo:**

1. **Dividir posición por resolución**: Convierte coordenadas de píxeles a índices de cuadrícula
2. **`floor()`**: Redondea hacia abajo para obtener índice entero
3. **`constrain()`**: Asegura que no salga de los límites del array
4. **`.copy()`**: Retorna una copia del vector (no el original)

**Ejemplo numérico:**
```
Agente en posición (100, 80)
Resolución = 20

Cálculo:
- column = floor(100 / 20) = floor(5) = 5
- row = floor(80 / 20) = floor(4) = 4

Resultado: field[5][4]
```

**Diagrama conceptual:**
```
Posición del agente (píxeles) → División por resolución → Índices de array
      (100, 80)               →      (5.0, 4.0)         →    [5][4]
```

<ins><strong>Parte B: ¿Cómo calcula la steering force?</strong></ins>

**Fórmula fundamental:**
```
steering_force = velocidad_deseada - velocidad_actual
```

**Implementación paso a paso:**

```javascript
// 1. Obtener vector deseado (unitario)
let desired = flow.lookup(this.position);

// 2. Escalar a velocidad máxima
desired.mult(this.maxspeed);
// Ahora 'desired' tiene la magnitud correcta

// 3. Calcular diferencia con velocidad actual
let steer = p5.Vector.sub(desired, this.velocity);
// steer = hacia_dónde_quiero_ir - hacia_dónde_voy

// 4. Limitar a fuerza máxima
steer.limit(this.maxforce);
// Impide giros instantáneos

// 5. Aplicar como fuerza
this.applyForce(steer);
```

**Ejemplo visual:**

```
Situación:
  Vector del campo:      →  (apunta derecha)
  Velocidad actual:      ↑  (va hacia arriba)
  Velocidad deseada:     →  (quiere ir a derecha con maxspeed)

Cálculo:
  Steering force:        ↘  (fuerza que lo "jala" hacia la derecha)

Resultado:
  El agente GRADUALMENTE gira hacia la derecha
  (no instantáneamente, gracias a maxforce)
```

**¿Por qué es importante limitar la fuerza?**

- Sin `limit(maxforce)`: El agente giraría instantáneamente → movimiento robótico
- Con `limit(maxforce)`: Giros suaves y naturales → movimiento orgánico

**Analogía del mundo real:**
Es como un auto que no puede girar 90° instantáneamente. Debe reducir velocidad y girar gradualmente según su "capacidad de maniobra" (maxforce).

**Flujo completo del movimiento**

```
1. Agente en posición (x, y)
2. lookup() → obtiene vector del campo en esa posición
3. Escala vector a maxspeed → velocidad deseada
4. Calcula steering = deseada - actual
5. Limita steering a maxforce
6. Aplica steering como fuerza
7. Actualiza aceleración
8. Actualiza velocidad (limitada a maxspeed)
9. Actualiza posición
10. Repite en siguiente frame
```

**Observación clave**

El agente NO salta directamente a seguir el vector del campo. En su lugar:
- **Percibe** el vector (lookup)
- **Desea** moverse en esa dirección (desired)
- **Calcula** la fuerza necesaria (steering)
- **Aplica gradualmente** esa fuerza (limitada por maxforce)

Esto crea movimiento autónomo e inteligente, no simplemente seguir un path predefinido.

</details>

4. Identifica parámetros clave: localiza en el código las variables que controlan aspectos importantes como:

   - La resolución del campo de flujo (el tamaño de las celdas de la cuadrícula).
   - La velocidad máxima (`maxspeed`) y la fuerza máxima (`maxforce`) de los agentes.


<details>
<summary>🎛️ Paso 4: Identificar parámetros clave</summary>

**Parámetros identificados**

| Parámetro | Valor actual | Ubicación | Efecto |
|-----------|--------------|-----------|--------|
| **resolution** | 20 | sketch.js:23 | Tamaño de celdas del campo |
| **maxspeed** | 2-5 (random) | sketch.js:27 | Velocidad del agente |
| **maxforce** | 0.1-0.5 (random) | sketch.js:27 | Capacidad de maniobra |
| **noise increment** | 0.1 | flowfield.js:32,34 | Suavidad del campo |
| **num vehicles** | 120 | sketch.js:25 | Cantidad de agentes |

<ins><strong>1. Resolución del campo de flujo</strong></ins>

**Ubicación en el código:**
```javascript
// En sketch.js, línea 23
flowfield = new FlowField(20);
```

**Qué controla:**
- El tamaño de cada celda de la cuadrícula
- Cuántos vectores componen el campo

**Efecto de diferentes valores:**

| Resolución | Columnas | Filas | Total vectores | Efecto visual |
|------------|----------|-------|----------------|---------------|
| 5 | 128 | 48 | 6144 | Muy detallado, campo complejo |
| 10 | 64 | 24 | 1536 | Detallado |
| 20 | 32 | 12 | 384 | Balance (valor actual) |
| 40 | 16 | 6 | 96 | Grueso, cambios bruscos |
| 60 | 10.6 | 4 | 42 | Muy grueso, poco detalle |

**Impacto en el comportamiento:**
- **Resolución baja** (5-10): Campo muy detallado, agentes siguen curvas complejas
- **Resolución alta** (40-60): Campo grueso, agentes hacen movimientos más amplios

<ins><strong>2. Velocidad máxima (maxspeed)</strong></ins>

**Ubicación en el código:**
```javascript
// En sketch.js, línea 27
new Vehicle(random(width), random(height), random(2, 5), random(0.1, 0.5))
//                                          ^^^^^^^^^^^^
//                                          maxspeed entre 2 y 5
```

**Qué controla:**
- Qué tan rápido puede moverse el agente
- La magnitud máxima del vector velocidad

**Relación con el código:**
```javascript
// En vehicle.js
this.velocity.limit(this.maxspeed);
```

**Efecto de diferentes valores:**

| maxspeed | Comportamiento |
|----------|----------------|
| 0.5 - 1 | Muy lento, movimiento contemplativo |
| 2 - 5 | Balance, movimiento natural (actual) |
| 10 - 15 | Rápido, enérgico |
| 20+ | Muy rápido, puede verse caótico |

<ins><strong>3. Fuerza máxima (maxforce)</strong></ins>

**Ubicación en el código:**
```javascript
// En sketch.js, línea 27
new Vehicle(random(width), random(height), random(2, 5), random(0.1, 0.5))
//                                                        ^^^^^^^^^^^^^^^^^
//                                                        maxforce entre 0.1 y 0.5
```

**Qué controla:**
- Qué tan rápido puede cambiar de dirección
- La "capacidad de maniobra" del agente
- La magnitud máxima de la steering force

**Relación con el código:**
```javascript
// En vehicle.js
steer.limit(this.maxforce);
```

**Efecto de diferentes valores:**

| maxforce | Comportamiento |
|----------|----------------|
| 0.01 - 0.05 | Giros muy lentos, inercia alta, movimiento "pesado" |
| 0.1 - 0.5 | Giros naturales (actual) |
| 1 - 2 | Muy maniobrable, giros cerrados |
| 5+ | Giros casi instantáneos, puede verse antinatural |

<ins><strong>4. Incremento de Perlin Noise</strong></ins>

**Ubicación en el código:**
```javascript
// En flowfield.js, líneas 32-34
yoff += 0.1;
xoff += 0.1;
```

**Qué controla:**
- Qué tan rápido cambia el Perlin noise entre celdas adyacentes
- La "suavidad" del campo de flujo

**Efecto de diferentes valores:**

| Incremento | Efecto en el campo |
|------------|-------------------|
| 0.01 | Muy suave, casi uniforme |
| 0.1 | Suave con variación (actual) |
| 0.5 | Cambios frecuentes |
| 1.0+ | Caótico, direcciones muy diferentes entre celdas |

<ins><strong></strong></ins>5. Número de vehículos

**Ubicación en el código:**
```javascript
// En sketch.js, línea 25
for (let i = 0; i < 120; i++) {
```

**Qué controla:**
- Cuántos agentes existen en la simulación

**Consideraciones:**
- Más agentes = visualización más densa del campo
- Afecta rendimiento computacional

**Relación entre parámetros**

**maxspeed vs maxforce:**
```
Alto maxspeed + Bajo maxforce = Rápido pero torpe (difícil de girar)
Bajo maxspeed + Alto maxforce = Lento pero ágil (gira fácilmente)
Alto maxspeed + Alto maxforce = Rápido y ágil (muy responsivo)
Bajo maxspeed + Bajo maxforce = Lento y torpe (movimiento "pesado")
```

**resolución vs incremento noise:**
```
Baja resolución + Alto incremento = Movimiento caótico
Alta resolución + Bajo incremento = Movimiento muy suave pero computacionalmente costoso
```


</details>


5. Experimenta con modificaciones: realiza al menos una de las siguientes modificaciones en el código, ejecuta y describe el efecto observado en el comportamiento de los agentes:
   - Cambia significativamente la forma en que se generan los vectores del campo (ej: usa una fórmula matemática diferente en lugar de noise(), o cambia drásticamente los parámetros de noise()).
   - Modifica sustancialmente la resolución del campo de flujo (hazla mucho más fina o mucho más gruesa).
   - Altera considerablemente maxspeed o maxforce de los agentes.

> [!NOTE]
> 🧐🧪✍️ Reporta en tu bitácora
>
> 1. Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.
> 2. Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.
> 3. Lista los parámetros clave identificados (resolución, maxspeed, maxforce).
> 4. Describe la modificación que realizaste al código y **explica detalladamente el efecto** que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.

<details>
<summary>🧪 Paso 5: Experimentación con modificaciones</summary>

<details>
  <summary>🧪 Modificación: Cambiar generación de vectores del campo</summary>

He implementado **4 modos diferentes** de generación de vectores, que se pueden cambiar presionando la tecla **'C'**:

**Modo 0: Perlin Noise (Original)**
```javascript
let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
```

**Comportamiento observado:**
- Movimiento orgánico y natural
- Patrones fluidos y continuos
- Los agentes forman "ríos" de movimiento
- Cada regeneración (click) crea un patrón completamente diferente

**Características:**
- Campo más natural y menos predecible
- Excelente para simular flujos naturales (viento, agua, etc.)
- Variación aleatoria controlada

---

**Modo 1: Espiral**
```javascript
let angle = atan2(j - this.rows/2, i - this.cols/2) + PI/4;
```

**Comportamiento observado:**
- Los agentes forman un patrón de **espiral** girando alrededor del centro del canvas
- Movimiento rotacional constante en sentido horario
- Todos los agentes convergen hacia un flujo circular
- Patrón muy predecible y simétrico

**Efecto visual:**
- Remolino o vórtice
- Movimiento hipnótico y ordenado
- Similar a agua drenando por un desagüe

**Aplicaciones:**
- Simular tornados o remolinos
- Efectos de agujero negro
- Patrones decorativos circulares

---

**Modo 2: Patrón de Ondas**
```javascript
let angle = sin(i * 0.2) * cos(j * 0.2) * TWO_PI;
```

**Comportamiento observado:**
- Los agentes siguen patrones de **ondas sinusoidales**
- Movimiento ondulante, como olas en el mar
- Crea áreas de movimiento caótico intercaladas con áreas organizadas
- Patrones geométricos repetitivos

**Efecto visual:**
- Movimiento serpenteante
- Zonas de convergencia y divergencia
- Sensación de vibración o pulsación

**Características:**
- Combina seno y coseno para crear interferencia
- Patrones más complejos y matemáticamente definidos
- Movimiento menos natural pero visualmente interesante

**Aplicaciones:**
- Arte generativo con patrones geométricos
- Simular ondas electromagnéticas
- Efectos visuales abstractos

---

**Modo 3: Circular**
```javascript
let angle = atan2(j - this.rows/2, i - this.cols/2);
```

**Comportamiento observado:**
- Los agentes se mueven en **círculos concéntricos** alrededor del centro
- Movimiento rotacional puro sin offset
- Similar al modo espiral pero sin el componente que los hace espiralar hacia adentro
- Orbitan alrededor del punto central

**Efecto visual:**
- Órbitas circulares perfectas
- Movimiento planetario
- Muy simétrico y ordenado

**Diferencia con modo espiral:**
- Espiral: `+ PI/4` hace que también se muevan hacia el centro
- Circular: Solo rotan sin acercarse o alejarse del centro

**Aplicaciones:**
- Simular órbitas planetarias
- Efectos de campo magnético
- Visualizaciones de fuerzas centrípetas

---

**Comparación entre modos**

| Modo | Predecibilidad | Naturalidad | Complejidad visual | Aplicación ideal |
|------|----------------|-------------|-------------------|------------------|
| **Noise** | Baja | Alta | Media | Flujos naturales |
| **Espiral** | Alta | Media | Media | Vórtices, remolinos |
| **Ondas** | Media | Baja | Alta | Arte generativo |
| **Circular** | Alta | Baja | Baja | Órbitas, campos |

**Código implementado**

He modificado el archivo `flowfield.js` para incluir:

1. **Propiedad de modo** en el constructor
2. **Switch statement** en `init()` para seleccionar entre modos
3. **Función `changeMode()`** para ciclar entre modos
4. **Función `getModeName()`** para mostrar el modo actual

Y en `sketch.js`:
- Agregué detección de tecla 'C' para cambiar modos
- Mostrar el nombre del modo actual en pantalla

**Fragmento de código clave:**
```javascript
// En flowfield.js
switch(this.mode) {
  case 0: // Perlin Noise (original)
    angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
    break;
  case 1: // Espiral
    angle = atan2(j - this.rows/2, i - this.cols/2) + PI/4;
    break;
  case 2: // Patrón de ondas
    angle = sin(i * 0.2) * cos(j * 0.2) * TWO_PI;
    break;
  case 3: // Circular
    angle = atan2(j - this.rows/2, i - this.cols/2);
    break;
}
```

**Impacto en el comportamiento colectivo**

**Observación general:**
El cambio en la generación de vectores tiene un **impacto dramático** en el comportamiento colectivo:

- **Noise**: Comportamiento emergente impredecible → agentes parecen explorar
- **Espiral/Circular**: Comportamiento emergente predecible → agentes parecen seguir reglas estrictas
- **Ondas**: Comportamiento emergente caótico → agentes parecen desorientados en algunas zonas

Esto demuestra el principio clave de la unidad: **reglas simples → comportamientos complejos**.

</details>


<details>
<summary>🧪 Modificación: Cambiar resolución del campo (Mención)</summary>

**Experimentación con resolución**

He probado diferentes valores de resolución para entender su impacto en el comportamiento del sistema.

**Valores probados**

**Resolución muy baja (5)**
```javascript
flowfield = new FlowField(5);
```

**Resultado:**
- El sistema se **lagea considerablemente**
- Se crean **6,144 vectores** (128 × 48)
- Los agentes responden a cambios muy sutiles en el campo
- Movimiento extremadamente detallado pero costoso computacionalmente

**Por qué lagea:**
- Demasiados vectores para calcular y procesar
- Mayor carga en `lookup()` y `show()`
- El navegador debe renderizar muchas más líneas en modo debug

**Resolución muy alta (>40)**
```javascript
flowfield = new FlowField(50);
```

**Resultado:**
- Funciona bien en términos de rendimiento
- Solo **85 vectores** (12.8 × 4.8)
- Los agentes hacen movimientos muy amplios y bruscos
- Se pierde la suavidad del movimiento

**Efecto visual:**
- Los agentes parecen "saltar" entre zonas
- Menor densidad de información direccional
- Patrones menos interesantes

**Conclusión sobre la resolución**

**Valor óptimo: 15-25**
- Balance entre detalle visual y rendimiento
- Movimiento suave sin sacrificar rendimiento
- Suficientes vectores para patrones interesantes

**¿Por qué es importante el diseño de resolución?**

**Consideraciones de diseño:**

1. **Rendimiento**: Más resolución = más cálculos = más lag
2. **Estética**: Debe haber suficientes vectores para crear patrones fluidos
3. **Comportamiento**: Resolución muy baja crea movimiento "pixelado"
4. **Hardware**: Depende del dispositivo donde se ejecuta

**Aplicación práctica:**
- Móviles: Resolución 25-40 (menos potentes)
- Desktop: Resolución 10-20 (más potentes)
- Proyecciones/instalaciones: Resolución ajustable según dimensiones

**Recomendación**

Mantener resolución en **20** como está en el ejemplo original es una excelente elección porque:
- ✅ Buen balance rendimiento/calidad
- ✅ Patrones visualmente interesantes
- ✅ Funciona en la mayoría de dispositivos
- ✅ Suficiente detalle sin complejidad excesiva

</details>


<details>
<summary>🧪 Modificación: Alterar maxspeed y maxforce</summary>

Experimentación con velocidad y fuerza

He creado diferentes configuraciones para entender cómo `maxspeed` y `maxforce` afectan el comportamiento de los agentes.

**Experimento 1: Agentes muy rápidos y maniobrables**

**Código modificado:**
```javascript
// En sketch.js, línea 27
new Vehicle(random(width), random(height), 10, 2)
//                                          ^^  ^
//                                          |   maxforce = 2
//                                          maxspeed = 10
```

**Comportamiento observado:**
- Los agentes se mueven **muy rápidamente** por el canvas
- Hacen **giros cerrados** sin perder velocidad
- Responden casi instantáneamente a cambios en el campo de flujo
- Movimiento enérgico y dinámico

**Efecto visual:**
- Parece una bandada de pájaros en vuelo rápido
- Mucha actividad y energía en el canvas
- Difícil seguir agentes individuales con la vista

**Características:**
- Alta responsividad al campo
- Sensación de urgencia o pánico
- Movimiento "deportivo" o acrobático

**Aplicaciones:**
- Simular pájaros asustados
- Efectos de energía o electricidad
- Partículas en explosión controlada

---

**Experimento 2: Agentes muy lentos y torpes**

**Código modificado:**
```javascript
// En sketch.js, línea 27
new Vehicle(random(width), random(height), 1, 0.01)
//                                          ^  ^^^^
//                                          |  maxforce = 0.01
//                                          maxspeed = 1
```

**Comportamiento observado:**
- Los agentes se mueven **muy lentamente**
- Hacen **giros amplios y lentos** - no pueden cambiar dirección rápidamente
- Tienen mucha **inercia** - resisten cambios de dirección
- Movimiento pesado y contemplativo

**Efecto visual:**
- Parece una corriente lenta de lava o miel
- Movimiento "submarino" o en cámara lenta
- Fácil seguir agentes individuales
- Sensación de peso y masa

**Características:**
- Baja responsividad al campo
- Gran inercia y momentum
- Movimiento elegante y fluido

**Aplicaciones:**
- Simular fluidos densos (lava, miel)
- Criaturas gigantes o pesadas
- Efectos de cámara lenta
- Ambiente onírico o meditativo

---

**Experimento 3: Rápidos pero torpes**

**Código modificado:**
```javascript
// En sketch.js, línea 27
new Vehicle(random(width), random(height), 8, 0.05)
//                                          ^  ^^^^
//                                          |  maxforce = 0.05 (muy bajo)
//                                          maxspeed = 8 (alto)
```

**Comportamiento observado:**
- Los agentes van **rápido** pero **no pueden girar bien**
- Hacen **curvas muy amplias**
- A menudo "se pasan" de la dirección deseada
- Movimiento errático e impreciso

**Efecto visual:**
- Como patinadores en hielo sin control
- Movimiento "resbaloso"
- Trayectorias que sobrepasan el objetivo
- Sensación de dificultad para controlar

**Analogía del mundo real:**
- Auto deportivo en pista de hielo
- Cohete sin buen sistema de dirección

---

**Experimento 4: Lentos pero ágiles**

**Código modificado:**
```javascript
// En sketch.js, línea 27
new Vehicle(random(width), random(height), 2, 1.5)
//                                          ^  ^^^
//                                          |  maxforce = 1.5 (alto)
//                                          maxspeed = 2 (bajo)
```

**Comportamiento observado:**
- Los agentes van **lento** pero pueden **girar en cualquier dirección fácilmente**
- Hacen **giros cerrados** inmediatos
- Movimiento muy preciso y controlado
- Siguen el campo de flujo con exactitud

**Efecto visual:**
- Como un robot de precisión
- Movimiento "inteligente" y deliberado
- Cambios de dirección casi instantáneos
- Muy responsivo pero calmado

**Analogía del mundo real:**
- Drone de precisión
- Pez pequeño en agua tranquila

---

**Análisis comparativo**

| Config | maxspeed | maxforce | Sensación | Mejor uso |
|--------|----------|----------|-----------|-----------|
| **Original** | 2-5 | 0.1-0.5 | Natural, equilibrado | Bandadas de pájaros |
| **Experimento 1** | 10 | 2 | Frenético, deportivo | Pájaros asustados, energía |
| **Experimento 2** | 1 | 0.01 | Pesado, lento | Fluidos densos, gigantes |
| **Experimento 3** | 8 | 0.05 | Resbaloso, errático | Patinaje, falta de control |
| **Experimento 4** | 2 | 1.5 | Preciso, robótico | Drones, precisión |

**Relación maxspeed vs maxforce**

**Fórmula conceptual:**
```
Agilidad = maxforce / maxspeed

Agilidad alta: Puede girar fácilmente relativo a su velocidad
Agilidad baja: Difícil girar relativo a su velocidad
```

**Ejemplos:**
- `maxspeed=10, maxforce=2`: Agilidad = 0.2 (moderada)
- `maxspeed=1, maxforce=0.01`: Agilidad = 0.01 (muy baja)
- `maxspeed=2, maxforce=1.5`: Agilidad = 0.75 (muy alta)

**Impacto en el comportamiento colectivo**

**Observación general:**

1. **Alta velocidad + Alta fuerza**:
   - Comportamiento colectivo caótico pero cohesivo
   - Forman remolinos rápidos
   - Alta densidad de actividad

2. **Baja velocidad + Baja fuerza**:
   - Comportamiento colectivo lento y disperso
   - Forman "ríos" amplios
   - Baja densidad de actividad

3. **Desbalance (alta velocidad, baja fuerza)**:
   - Comportamiento colectivo desorganizado
   - Agentes se "pierden" del flujo
   - Patrones erráticos

4. **Balance (moderada velocidad, moderada fuerza)**:
   - Comportamiento colectivo armonioso
   - Patrones emergentes claros
   - Visualmente más atractivo

**Código recomendado para diferentes efectos**

**Para energía y caos:**
```javascript
new Vehicle(random(width), random(height), 12, 3)
```

**Para calma y fluidez:**
```javascript
new Vehicle(random(width), random(height), 1.5, 0.05)
```

**Para balance natural (original):**
```javascript
new Vehicle(random(width), random(height), random(2, 5), random(0.1, 0.5))
```

**Conclusión**

Los parámetros `maxspeed` y `maxforce` son fundamentales para el "carácter" de los agentes:
- **maxspeed**: Define la "energía" del sistema
- **maxforce**: Define la "inteligencia" o capacidad de respuesta
- **Su relación**: Define la "personalidad" del movimiento

Esta experimentación demuestra que el mismo algoritmo puede generar comportamientos totalmente diferentes simplemente ajustando dos parámetros numéricos.

</details>


</details>

<details>
<summary>🔗 Código de experimentación modificado</summary>

**Archivos modificados a partir del original**

He realizado modificaciones en dos archivos principales para implementar las experimentaciones:

**1. flowfield.js**

**Cambios realizados:**
- Agregada propiedad `mode` para controlar el tipo de generación
- Modificada función `init()` con switch statement para múltiples modos
- Agregada función `changeMode()` para ciclar entre modos
- Agregada función `getModeName()` para obtener nombre del modo actual

**Código clave agregado:**
```javascript
// Constructor
this.mode = 0; // 0=noise, 1=espiral, 2=ondas, 3=circular

// En init()
switch(this.mode) {
  case 0: // Perlin Noise (original)
    angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
    break;
  case 1: // Espiral
    angle = atan2(j - this.rows/2, i - this.cols/2) + PI/4;
    break;
  case 2: // Patrón de ondas
    angle = sin(i * 0.2) * cos(j * 0.2) * TWO_PI;
    break;
  case 3: // Circular
    angle = atan2(j - this.rows/2, i - this.cols/2);
    break;
}

// Funciones nuevas
changeMode() {
  this.mode = (this.mode + 1) % 4;
  this.init();
}

getModeName() {
  const modes = ['Perlin Noise', 'Espiral', 'Patrón de Ondas', 'Circular'];
  return modes[this.mode];
}
```

**2. sketch.js**

**Cambios realizados:**
- Actualizado texto de instrucciones para incluir tecla 'C'
- Agregado manejo de tecla 'C' en `keyPressed()`
- Agregado display del modo actual en el canvas

**Código clave agregado:**
```javascript
// En setup()
let text = createP(
  "Hit space bar to toggle debugging lines.<br>" +
  "Click the mouse to generate a new flow field.<br>" +
  "Press 'C' to change flow field mode."
);

// En draw()
fill(0);
noStroke();
textAlign(LEFT);
textSize(14);
text('Modo: ' + flowfield.getModeName(), 10, 20);

// En keyPressed()
if (key == "c" || key == "C") {
  flowfield.changeMode();
}
```

</details>


**Controles del sketch:**
- **Barra espaciadora**: Toggle debug lines (mostrar/ocultar vectores)
- **Click mouse**: Regenerar campo de flujo (solo afecta modo Perlin Noise)
- **Tecla 'C'**: Cambiar entre modos (Noise → Espiral → Ondas → Circular → Noise...)

[Ver en vivo en p5js](https://editor.p5js.org/DanieLudens/sketches/Um319xIwj)

<img width="400" src="https://github.com/user-attachments/assets/59af22b9-4aa9-4efe-bc56-10feef51f1f4">


### Actividad 04

> [!NOTE]
> 🎯 Enunciado
> Ahora analizaremos el segundo algoritmo: el comportamiento de enjambre (Flocking), famoso por simular el movimiento coordinado de pájaros o peces. Nos basaremos nuevamente en “The Nature of Code” para entender las tres reglas básicas que lo gobiernan.

> [!TIP]
> Recursos
>
> Libro “The Nature of Code” (TNoC) de Daniel Shiffman: [capítulo 5, sección “Flocking”](https://natureofcode.com/autonomous-agents/#flocking) (y ejemplos de código asociados).
> El código fuente del ejemplo de Flocking de TNoC.

#### Pasos:

1. **Ejecuta el ejemplo:** ejecuta el código del ejemplo principal de Flocking de TNoC en p5.js. Observa el movimiento colectivo de los “boids” (agentes).

<details>
<summary>📋 Paso 1: Ejecutar el ejemplo</summary>

**Comportamiento observado**

**Configuración inicial:**
- **120 boids** (agentes) representados como triángulos
- Todos inician en el centro del canvas `(width/2, height/2)`
- Cada boid tiene velocidad inicial aleatoria

**Movimiento colectivo:**
- Los boids comienzan dispersándose desde el centro
- Gradualmente forman grupos cohesivos
- Se mueven como una "bandada" o "cardumen"
- El movimiento es fluido y orgánico
- No hay líder - el comportamiento emerge de las interacciones locales

**Características del comportamiento:**
- **Cohesión**: Los boids tienden a agruparse
- **Separación**: Mantienen espacio personal, evitan colisiones
- **Alineación**: Se mueven en direcciones similares dentro del grupo
- **Wraparound**: Los boids que salen por un borde aparecen por el opuesto

**Controles interactivos**

- **Arrastrar el mouse**: Agrega nuevos boids en la posición del cursor
- Permite crear múltiples bandadas o aumentar la densidad del enjambre

**Observaciones**

El comportamiento interesante:
- Con solo 3 reglas simples, los boids generan patrones
- Se forman "bandadas" que se dividen y se vuelven a unir
- El movimiento parece inteligente y coordinado
- No hay comunicación global - cada boid solo "ve" a sus vecinos cercanos
- Es imposible predecir el patrón exacto, pero el comportamiento general es consistente

**Diferencia con Flow Fields:**
- Flow Fields: Los agentes siguen un campo externo predefinido
- Flocking: Los agentes responden a otros agentes (interacción social)

</details>

2. Identifica las tres reglas: en el código de la clase del agente (ej: Boid), localiza las funciones que implementan las tres reglas fundamentales del Flocking:

   - **Separación (Separation):** evitar el hacinamiento con vecinos cercanos.
   - **Alineación (Alignment):** dirigirse en la misma dirección promedio que los vecinos cercanos.
   - **Cohesión (Cohesion):** moverse hacia la posición promedio de los vecinos cercanos.


<details>
<summary>🔍 Paso 2: Identificar las tres reglas</summary>

Las tres reglas fundamentales del Flocking están implementadas en la clase `Boid` en boid.js.

**Ubicación en el código**

La función `flock()` coordina las tres reglas:

```javascript
// En boid.js, líneas 31-43
flock(boids) {
  let sep = this.separate(boids); // Separation
  let ali = this.align(boids);    // Alignment
  let coh = this.cohere(boids);   // Cohesion

  // Arbitrarily weight these forces
  sep.mult(1.5);
  ali.mult(1.0);
  coh.mult(1.0);

  // Add the force vectors to acceleration
  this.applyForce(sep);
  this.applyForce(ali);
  this.applyForce(coh);
}
```

---

<ins><strong>Regla 1: Separación (Separation)</strong></ins>

**Ubicación**: boid.js:95-126

**Nombre de la función**: `separate(boids)`

**Objetivo**: Evitar el hacinamiento con vecinos cercanos - mantener espacio personal

**Parámetro clave**:
```javascript
let desiredSeparation = 25; // píxeles
```

**Lógica general**:
1. Revisa todos los boids del sistema
2. Identifica cuáles están demasiado cerca (distancia < 25)
3. Para cada vecino cercano:
   - Calcula un vector que apunta **alejándose** del vecino
   - Normaliza el vector
   - Lo pondera por distancia (más cerca = mayor fuerza)
4. Promedia todos estos vectores de escape
5. Convierte el promedio en una steering force

**Pseudocódigo**:
```
Para cada boid en el sistema:
  Si distancia > 0 Y distancia < 25:
    Vector_escape = mi_posición - posición_vecino
    Normalizar vector_escape
    Dividir por distancia (más cerca = más fuerte)
    Sumar a acumulador
    Contar++

Promedio = acumulador / contador
Steering = (Promedio normalizado × maxspeed) - velocidad_actual
Limitar steering a maxforce
```

---

<ins><strong>Regla 2: Alineación (Alignment)</strong></ins>

**Ubicación**: boid.js:128-151

**Nombre de la función**: `align(boids)`

**Objetivo**: Dirigirse en la misma dirección promedio que los vecinos cercanos

**Parámetro clave**:
```javascript
let neighborDistance = 50; // píxeles
```

**Lógica general**:
1. Revisa todos los boids del sistema
2. Identifica vecinos dentro del radio de percepción (distancia < 50)
3. Para cada vecino:
   - Suma su **velocidad** (no posición)
4. Calcula el promedio de velocidades
5. Convierte ese promedio en una steering force

**Pseudocódigo**:
```
Para cada boid en el sistema:
  Si distancia > 0 Y distancia < 50:
    Sumar velocidad del vecino
    Contar++

Si hay vecinos:
  Promedio = suma_velocidades / contador
  Normalizar promedio
  Escalar a maxspeed
  Steering = promedio - velocidad_actual
  Limitar steering a maxforce
Sino:
  Steering = (0, 0)
```

**Diferencia clave**: Se promedian las **velocidades** (dirección de movimiento), no las posiciones.

---

<ins><strong>Regla 3: Cohesión (Cohesion)</strong></ins>

**Ubicación**: boid.js:153-173

**Nombre de la función**: `cohere(boids)`

**Objetivo**: Moverse hacia la posición promedio (centro de masa) de los vecinos cercanos

**Parámetro clave**:
```javascript
let neighborDistance = 50; // píxeles
```

**Lógica general**:
1. Revisa todos los boids del sistema
2. Identifica vecinos dentro del radio de percepción (distancia < 50)
3. Para cada vecino:
   - Suma su **posición**
4. Calcula el promedio de posiciones (centro del grupo local)
5. Usa la función `seek()` para dirigirse hacia ese punto

**Pseudocódigo**:
```
Para cada boid en el sistema:
  Si distancia > 0 Y distancia < 50:
    Sumar posición del vecino
    Contar++

Si hay vecinos:
  Centro_del_grupo = suma_posiciones / contador
  Steering = seek(centro_del_grupo)
Sino:
  Steering = (0, 0)
```

**Diferencia clave**: Se promedian las **posiciones**, luego se hace `seek()` hacia ese punto.

---

**Función auxiliar: seek()**

**Ubicación**: boid.js:58-67

Esta función es usada por `cohere()` y calcula una steering force hacia un objetivo:

```javascript
seek(target) {
  let desired = p5.Vector.sub(target, this.position);
  desired.normalize();
  desired.mult(this.maxspeed);
  let steer = p5.Vector.sub(desired, this.velocity);
  steer.limit(this.maxforce);
  return steer;
}
```

Es la misma fórmula de steering force que vimos en Flow Fields: `deseada - actual`.

---

**Resumen comparativo de las 3 reglas**

| Regla | Qué promedia | Resultado | Efecto |
|-------|--------------|-----------|--------|
| **Separación** | Vectores de escape | Huir del centro local | Dispersión |
| **Alineación** | Velocidades | Moverse en misma dirección | Sincronización |
| **Cohesión** | Posiciones | Ir hacia centro local | Agrupación |

**Interacción entre reglas**

Las 3 reglas trabajan en **tensión**:
- **Separación vs Cohesión**: Una empuja hacia afuera, otra hacia adentro → mantiene distancia óptima
- **Alineación**: Sincroniza direcciones → crea movimiento coordinado
- **Resultado**: Balance dinámico que produce comportamiento de bandada

</details>

  
3. **Explica las reglas:** para cada una de las tres reglas, explica con tus propias palabras:

   - ¿Cuál es el objetivo de la regla?
   -  ¿Cómo calcula el agente la fuerza de dirección correspondiente? (describe la lógica general, ej: “Calcula un vector apuntando lejos de los vecinos demasiado cercanos”).


<details>
<summary>💡 Paso 3: Explicar las reglas con mis palabras</summary>

**Regla 1: Separación (Separation)**

**¿Cuál es el objetivo?**

Evitar que los boids choquen entre sí. Es como el "espacio personal" en una multitud - nadie quiere que alguien esté demasiado cerca. Esta regla hace que cada boid mantenga una distancia mínima con sus vecinos.

**¿Cómo calcula la fuerza de dirección?**

El boid mira a su alrededor y dice: "¿Quién está demasiado cerca de mí?" (distancia < 25 píxeles). Para cada vecino invasor, calcula un vector que apunta **alejándose** de ese vecino. Si hay múltiples vecinos cercanos, promedia todos los vectores de escape. Además, aplica una regla inteligente: **cuanto más cerca está el vecino, más fuerte es la fuerza de escape** (divide por la distancia).

**Analogía**: Es como cuando caminas en una multitud - automáticamente te alejas de personas que están muy cerca.

**Efecto visual**: Los boids nunca se amontonan ni chocan, mantienen un "colchón" de espacio.

---

**Regla 2: Alineación (Alignment)**

**¿Cuál es el objetivo?**

Moverse en la misma dirección que tus vecinos. Es como cuando corres con un grupo - naturalmente ajustas tu dirección para ir hacia donde va el grupo. Esta regla crea la sincronización del movimiento.

**¿Cómo calcula la fuerza de dirección?**

El boid observa a todos sus vecinos cercanos (dentro de 50 píxeles) y pregunta: "¿Hacia dónde se están moviendo ellos?" Suma las **velocidades** (no posiciones) de todos los vecinos, calcula el promedio, y genera una steering force para que su velocidad coincida con ese promedio.

**Analogía**: Es como un corredor en el tour de francia que ajusta su ritmo y dirección para coincidir con el grupo cercano.

**Efecto visual**: Todos los boids en un grupo apuntan y se mueven en direcciones similares, como peces en un cardumen.

---

**Regla 3: Cohesión (Cohesion)**

**¿Cuál es el objetivo?**

Mantenerse cerca del grupo. Es el instinto de "no quiero quedarme solo". Esta regla atrae a los boids hacia el centro de su grupo local, evitando que se dispersen completamente.

**¿Cómo calcula la fuerza de dirección?**

El boid mira a sus vecinos cercanos (dentro de 50 píxeles) y calcula la **posición promedio** de todos ellos - el "centro de masa" del grupo local. Luego usa una steering force para dirigirse suavemente hacia ese punto central, como si hubiera un imán débil atrayéndolo hacia el grupo.

**Analogía**: Es como cuando las crias de pato, sientes el impulso de volver hacia donde está el grupo cuando se dan cuenta que estan alejados.

**Efecto visual**: Los boids forman grupos compactos en lugar de dispersarse por todo el canvas.

---

**La magia de la interacción**

Lo fascinante es que estas 3 reglas simples **compiten** entre sí:

1. **Cohesión** dice: "¡Ven aquí, acércate al grupo!" → Atracción
2. **Separación** dice: "¡No tan cerca!" → Repulsión
3. **Alineación** dice: "¡Vamos todos en esta dirección!" → Coordinación

El balance entre estas fuerzas crea un comportamiento emergente complejo:
- No se amontonan (separación evita colisiones)
- No se dispersan (cohesión mantiene el grupo)
- Se mueven coordinadamente (alineación sincroniza)

**Resultado**: Una bandada que se comporta como un organismo vivo, sin líder, sin plan central, solo reglas locales simples.

**Observación personal**

Al ejecutar el ejemplo, noté que:
- Cuando hay pocos boids, cada uno tiene más "libertad" y el movimiento es menos coordinado
- Cuando hay muchos boids, se forman grupos más compactos y el movimiento es más fluido
- Los grupos pueden dividirse y reunirse dinámicamente
- A veces se forman "mini bandadas" separadas que eventualmente se fusionan

Esto demuestra el **comportamiento emergente**: propiedades del sistema que no están programadas explícitamente, sino que surgen de la interacción de componentes simples.

</details>

  
4. **Identifica parámetros clave:** localiza en el código las variables que controlan:
   - El radio (o distancia) de percepción (`perceptionRadius` o similar) que define quiénes son los “vecinos”. A veces también hay un ángulo de percepción.
   - Los pesos o multiplicadores que determinan la influencia relativa de cada una de las tres reglas al combinarlas.
   - La velocidad máxima (`maxspeed`) y la fuerza máxima (`maxforce`) de los agentes (similar a Flow Fields).


<details>
<summary>🎛️ Paso 4: Identificar parámetros clave</summary>


**Parámetros clave**

| Parámetro | Valor actual | Ubicación | Efecto principal |
|-----------|--------------|-----------|------------------|
| **desiredSeparation** | 25 | boid.js:96 | Espacio personal entre boids |
| **neighborDistance** | 50 | boid.js:131, 156 | Radio de percepción social |
| **Peso Separación** | 1.5 | boid.js:36 | Prioridad de evitar colisiones |
| **Peso Alineación** | 1.0 | boid.js:37 | Prioridad de sincronizar dirección |
| **Peso Cohesión** | 1.0 | boid.js:38 | Prioridad de mantenerse en grupo |
| **maxspeed** | 3 | boid.js:14 | Velocidad máxima |
| **maxforce** | 0.05 | boid.js:15 | Capacidad de maniobra |
| **num boids** | 120 | sketch.js:17 | Cantidad de agentes |

---

**1. Radio de percepción (Perception Radius)**

Los boids tienen diferentes radios de percepción para diferentes comportamientos:

**Para Separación:**

**Ubicación**: boid.js:96
```javascript
let desiredSeparation = 25; // píxeles
```
- **Qué define**: Quiénes son vecinos "demasiado cercanos"
- **Efecto**: Qué tan apretado o espaciado está el grupo
- **Más pequeño** (10-15): Boids pueden estar muy juntos, casi tocándose
- **Más grande** (50+): Boids mantienen mucha distancia, grupos dispersos

**Para Alineación y Cohesión:**

**Ubicación**: boid.js:131 y boid.js:156
```javascript
let neighborDistance = 50; // píxeles
```
- **Qué define**: Quiénes son vecinos "cercanos" para alinearse y cohesionarse
- **Efecto**: Qué tan "consciente" es cada boid del grupo
- **Más pequeño** (20-30): Solo responde a vecinos inmediatos, grupos fragmentados
- **Más grande** (100+): Responde a muchos boids, comportamiento muy cohesivo

**Nota importante**: En este ejemplo, `desiredSeparation` y `neighborDistance` son constantes locales en cada función. Para experimentar fácilmente, se podrían hacer propiedades de la clase.

---

**2. Pesos de las reglas (Rule Weights)**

**Ubicación**: boid.js:36-38
```javascript
sep.mult(1.5);  // Peso de Separación
ali.mult(1.0);  // Peso de Alineación
coh.mult(1.0);  // Peso de Cohesión
```

**Qué controlan**: La influencia relativa de cada regla en el comportamiento final

**Valores actuales**:
- Separación: **1.5** (más importante)
- Alineación: **1.0** (importancia media)
- Cohesión: **1.0** (importancia media)

**Interpretación**: La separación tiene 50% más influencia que las otras reglas, lo que significa que evitar colisiones es prioritario.

**Efectos de diferentes configuraciones**:

| Separación | Alineación | Cohesión | Comportamiento resultante |
|------------|------------|----------|---------------------------|
| **1.5** | **1.0** | **1.0** | Balance (actual) - grupos cohesivos con buen espacio |
| 3.0 | 1.0 | 1.0 | Muy disperso - boids se evitan mucho |
| 0.1 | 1.0 | 1.0 | Amontonamiento - boids se chocan |
| 1.0 | 3.0 | 1.0 | Muy sincronizado - todos van juntos en línea |
| 1.0 | 0.1 | 1.0 | Caótico - grupo compacto pero sin dirección clara |
| 1.0 | 1.0 | 3.0 | Muy compacto - grupo apretado hacia el centro |
| 1.0 | 1.0 | 0.1 | Muy disperso - boids se alejan del grupo |

---

**3. Velocidad máxima (maxspeed)**

**Ubicación**: boid.js:14

```javascript
this.maxspeed = 3; // píxeles por frame
```

**Qué controla**: Qué tan rápido puede moverse cada boid

**Efecto**:
- **Valor bajo** (1-2): Movimiento lento y contemplativo, fácil de observar
- **Valor actual** (3): Balance entre velocidad y suavidad
- **Valor alto** (8-10): Movimiento rápido y enérgico, difícil de seguir

**Relación con el código**:
```javascript
// En boid.js:50
this.velocity.limit(this.maxspeed);
```

---

**4. Fuerza máxima (maxforce)**

**Ubicación**: boid.js:15

```javascript
this.maxforce = 0.05; // aceleración máxima
```

**Qué controla**: Qué tan rápido puede cambiar de dirección (capacidad de maniobra)

**Efecto**:
- **Valor bajo** (0.01-0.03): Giros lentos, movimiento con inercia, más "realista"
- **Valor actual** (0.05): Buen balance
- **Valor alto** (0.2-0.5): Giros bruscos, movimiento "nervioso"

**Relación con el código**:
```javascript
// Usado en múltiples lugares, ej: boid.js:123
steer.limit(this.maxforce);
```

---

**5. Número de boids**

**Ubicación**: sketch.js:17

```javascript
for (let i = 0; i < 120; i++) {
```

**Qué controla**: Cuántos agentes hay en el sistema

**Efectos**:

| Cantidad | Comportamiento | Rendimiento |
|----------|----------------|-------------|
| 10-30 | Grupos pequeños, mucha libertad individual | Excelente |
| 50-150 | Comportamiento de bandada claro (actual: 120) | Bueno |
| 200-500 | Súper bandadas densas y complejas | Regular |
| 1000+ | Comportamiento masivo pero puede lagear | Malo |

**Consideración**: Cada boid compara su posición con TODOS los demás boids, por lo que la complejidad es O(n²). Muchos boids = mucho cálculo.

---

**6. Tamaño del boid (r)**

**Ubicación**: boid.js:13

```javascript
this.r = 3.0; // radio del triángulo
```

**Qué controla**: Tamaño visual del boid

**Efecto**:
- Solo visual, no afecta la física
- Boids más grandes son más fáciles de ver individualmente
- Boids más pequeños crean efecto de "enjambre" más denso visualmente

---

**Relaciones importantes entre parámetros**

**Radio de percepción vs Número de boids**:
- Radio grande + Muchos boids = Cada boid responde a muchos vecinos → comportamiento muy cohesivo
- Radio pequeño + Muchos boids = Cada boid responde a pocos vecinos → grupos fragmentados

**Separación vs Cohesión**:
- Separación > Cohesión = Grupos dispersos
- Cohesión > Separación = Grupos apretados (pueden colisionar)
- Balance = Distancia óptima

**maxspeed vs maxforce**:
- Como en Flow Fields, la relación determina la "agilidad"
- Alto maxspeed + Bajo maxforce = Movimiento con mucha inercia
- Bajo maxspeed + Alto maxforce = Movimiento preciso y respondiente

</details>


5. **Experimenta con modificaciones:** realiza al menos **una** de las siguientes modificaciones en el código, ejecuta y describe el efecto observado en el comportamiento colectivo del enjambre:

   - Cambia drásticamente el peso de una de las reglas (ej: pon la cohesión a cero, o la separación muy alta).
   - Modifica significativamente el radio de percepción (hazlo muy pequeño o muy grande).
   - Introduce un objetivo (`target`) que todos los boids intenten seguir (usando una fuerza de `seek`) además de las reglas de flocking, y ajusta su influencia.
  
> [!NOTE]
> 🧐🧪✍️ Reporta en tu bitácora
>
> 1. Explica con tus palabras el objetivo y la lógica general de cálculo de cada una de las tres reglas de Flocking (Separación, Alineación, Cohesión).
> 2. Lista los parámetros clave identificados (radio de percepción, pesos de las reglas, maxspeed, maxforce).
> 3. Describe la modificación que realizaste al código y **explica detalladamente el efecto** que tuvo en el comportamiento colectivo del enjambre (¿Se dispersan? ¿Forman grupos compactos? ¿se mueven caóticamente?). Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.


<details>
<summary>🧪 Paso 5: Experimentación con modificaciones</summary>

Cambiar peso de las reglas para observar cómo afectan el comportamiento colectivo del enjambre.

---

**Experimento 1: Separación muy alta**

**Código modificado** en boid.js:36-38:
```javascript
sep.mult(5.0);  // Separación MUY alta
ali.mult(1.0);
coh.mult(1.0);
```

**Comportamiento observado**:
- Los boids se **dispersan** significativamente
- Mantienen mucha distancia entre sí
- Los grupos se **fragmentan** en mini bandadas
- Movimiento menos cohesivo, más individualista
- Algunos boids quedan "solos" vagando

**Efecto visual**:
- Parece una nube de partículas expandiéndose
- Rara vez forman grupos densos
- Más espacio blanco entre boids

**Interpretación**:
Al priorizar fuertemente la separación, cada boid "valora más" su espacio personal que mantenerse cerca del grupo. Es como personas que son muy introvertidas - prefieren la distancia.

**Aplicaciones potenciales**:
- Simular partículas que se repelen (electrones)
- Movimiento de individuos en espacios abiertos
- Comportamiento territorial de animales

---

**Experimento 2: Cohesión muy alta**

**Código modificado**:
```javascript
sep.mult(1.5);
ali.mult(1.0);
coh.mult(5.0);  // Cohesión MUY alta
```

**Comportamiento observado**:
- Los boids forman **grupos extremadamente compactos**
- Se "amontonan" en el centro del grupo
- El grupo actúa como una **masa unificada**
- A veces se producen pequeñas colisiones visuales
- Movimiento más lento debido a la congestión

**Efecto visual**:
- Parece una gota de tinta moviéndose
- Masa densa y oscura de boids
- Difícil distinguir individuos

**Interpretación**:
Al priorizar fuertemente la cohesión, cada boid "necesita" estar cerca del centro del grupo. Es como un grupo de personas en pánico apiñándose.

**Aplicaciones potenciales**:
- Simular multitudes en pánico
- Comportamiento de enjambres de insectos muy densos
- Células agrupándose

---

**Experimento 3: Alineación muy alta**

**Código modificado**:
```javascript
sep.mult(1.5);
ali.mult(5.0);  // Alineación MUY alta
coh.mult(1.0);
```

**Comportamiento observado**:
- Los boids forman **líneas y formaciones alargadas**
- Se mueven de manera **extremadamente sincronizada**
- Todos apuntan en casi la misma dirección
- El grupo se ve como una **flecha o corriente**
- Cambios de dirección son colectivos y dramáticos

**Efecto visual**:
- Parece un río fluyendo
- Formación en "V" como aves migratorias
- Movimiento muy fluido y direccional

**Interpretación**:
Al priorizar fuertemente la alineación, cada boid "imita" fuertemente la dirección de sus vecinos. Es como corredores en una carrera siguiendo al pelotón.

**Aplicaciones potenciales**:
- Simular aves migratorias
- Tráfico vehicular en autopista
- Movimiento de multitudes en una dirección

---

**Experimento 4: Cohesión cero**

**Código modificado**:
```javascript
sep.mult(1.5);
ali.mult(1.0);
coh.mult(0.0);  // Cohesión DESACTIVADA
```

**Comportamiento observado**:
- Los boids se **dispersan completamente**
- No hay tendencia a formar grupos
- Cada boid va "a su aire"
- Movimiento caótico y desorganizado
- Los boids llenan todo el canvas uniformemente

**Efecto visual**:
- Parece partículas brownianas
- No hay grupos reconocibles
- Distribución aleatoria por el espacio

**Interpretación**:
Sin cohesión, no hay "atracción" al grupo. La separación los empuja apart, pero nada los mantiene juntos. Es como personas que no se conocen en un espacio público.

**Aplicaciones potenciales**:
- Simular moléculas de gas
- Individuos sin conexión social
- Exploración aleatoria del espacio

---

**Experimento 5: Separación cero**

**Código modificado**:
```javascript
sep.mult(0.0);  // Separación DESACTIVADA
ali.mult(1.0);
coh.mult(1.0);
```

**Comportamiento observado**:
- Los boids se **amontonan y colisionan**
- No respetan espacio personal
- Se forma una **masa caótica** en el centro
- Movimiento errático dentro del grupo
- Algunos boids "rebotan" entre sí visualmente

**Efecto visual**:
- Parece una pelota de boids vibrando
- Superposición visual de triángulos
- Movimiento tembloroso

**Interpretación**:
Sin separación, los boids pierden su sentido de espacio personal. La cohesión los atrae pero nada los mantiene separados. Es como una multitud en concierto sin control.

**Aplicaciones potenciales**:
- Simular partículas que se atraen (moléculas)
- Multitudes extremadamente densas
- Comportamiento caótico

---

**Experimento 6: Balance diferente (comportamiento más realista)**

**Código modificado**:
```javascript
sep.mult(2.0);  // Separación ligeramente más alta
ali.mult(1.5);  // Alineación media-alta
coh.mult(0.8);  // Cohesión ligeramente más baja
```

**Comportamiento observado**:
- Comportamiento **muy natural** similar a pájaros reales
- Buenos espacios entre boids
- Movimiento fluido y coordinado
- Grupos cohesivos pero no apretados
- Transiciones suaves

**Efecto visual**:
- Parece una bandada de pájaros real
- Elegante y orgánico
- Placentero visualmente

**Interpretación**:
Este balance prioriza ligeramente evitar colisiones y moverse juntos en la misma dirección, mientras reduce un poco la atracción al centro. Produce el comportamiento más "natural".

</details>

[Ver en vivo en P5js](https://editor.p5js.org/DanieLudens/sketches/ZxA4EIz7r)

|Experimento|Info|Muestra|
|:---:|:--|:---|
|Balance|Comportamiento muy natural|<img width="500" src="https://github.com/user-attachments/assets/11e56ef5-e76a-4c43-a1c0-c4e9cba52aa2">|
|Separacion Alta|Se dispersan significativamente|<img width="500" src="https://github.com/user-attachments/assets/6f0b4b8b-3874-4bba-85d5-0a0bc7310fda">|
|Separación Cero|Se amontonan y colisionan|<img width="500" src="https://github.com/user-attachments/assets/1e39e8aa-00f6-4ff6-94fe-851e814a6aef">|
|Cohesión Alta|Forman grupos muy compactos|<img width="500" src="https://github.com/user-attachments/assets/8364f841-78bc-4353-9e8d-1b1c21b80164">|
|Coheción Cero|Se dispersan completamente|<img width="500" src="https://github.com/user-attachments/assets/048cfe1b-81e3-4053-bcf1-5fdcecac02bf">|
|Alineación Alta|Formaciones alargadas|<img width="500" src="https://github.com/user-attachments/assets/ed26e0ae-226b-456a-95f4-9b3a94ea84fe">|
|Alineación Cero|Sin Formación aparente|<img width="500" src="https://github.com/user-attachments/assets/27fb3a87-9b41-47f3-af2b-11e0163b563d">|

## Apply: Aplicación 🛠

> [!NOTE]
> Creación generativa con un giro inesperado
>
> Ahora que entiendes los algoritmos, es tu turno de crear. Aplicarás uno de ellos (flow fields o flocking) para generar una pieza de arte interactivo que permita visualizar un tema musical de tu elección. La interacción del usuario debe influir en el comportamiento de los agentes.

### Actividad 05

1. Elige un tema musical que te inspire.
2. Diseña una pieza de arte generativo que utilice el algoritmo de flow fields y/o flocking.
3. Vas a visualizar el tema musical y además vas a “tocar” las visuales, es decir, tu pieza de arte debe ser interactiva y debe permitir la interpretación en tiempo real de las visuales como si fuera un instrumento más que acompaña de manera coherente el tema musical.

> [!NOTE]
> 🧐🧪✍️ Reporta en tu bitácora
>
> 1. Documenta todo el proceso de diseño y creación en tu bitácora, incluyendo bocetos y decisiones de diseño.
> 2. El código fuente completo de tu sketch en p5.js.
> 3. Un enlace a tu sketch en el editor de p5.js.
> 4. Capturas de pantalla mostrando tu pieza en acción.


## Evidencias 🗂️

> [!NOTE]
> RUBRICA!
>
> - Recuerda que la bitácora se cierra 10 minutos antes de iniciar la sesión de evaluación de la unidad (segunda sesión de la segunda semana). plasma en la bitácora. La bitácora no es un resultado que se llena a última hora.
> - Si no realizas la autoevaluación tu nota será 0.
> - Si una actividad no está COMPLETA debes multiplicar la nota de esa actividad por el porcentaje de avance que tengas.
> - Puedes usar IA generativa para generar código, pero NO para DISEÑAR.
> 
> Rúbrica de evaluación del proceso
>
> 5: realicé las 5 actividades completas y la autoevaluación.
> 4: realicé 4 actividades completas y la autoevaluación.
> 3: realicé 3 actividades completas y la autoevaluación.
> 2: realicé 2 actividades completas y la autoevaluación.
> 1: realicé 1 actividad completa y la autoevaluación.
> 0: no realicé ninguna actividad o no realicé la autoevaluación.

> [!WARNING]
> **EVIDENCIAS EN BITÁCORA**
> 1. Realiza las actividades propuestas en esta unidad y documenta todo el proceso en tu bitácora.
> 2. Realiza la autoevaluación indicando:
>    - Tu nota propuesta.
>    - La defensa de esa nota para cada actividad.


## Reflect: Consolidación y metacognición 🤔

1. Explica en tus propias propias palabras qué es un agente autónomo y cómo puede contribuir al arte generativo.


2. ¿Qué es el comportamiento emergente y cómo se manifiesta en los algoritmos de flow fields y flocking?

3. Para los más curisos. ¿Puedo aplicar lo que aprendí en esta unidad en Blender? Te dejo un [video](https://youtu.be/eqLA7oJkLyM?si=s97HHI8vErOxSDO5) para que te antojes y si creas algo en Blender ¿Me muestras?

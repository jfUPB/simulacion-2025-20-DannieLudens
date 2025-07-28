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

<details>
  <summary>Concepto obra generativa 1</summary>

1. Inspiracion basada en la observacion de la naturaleza y la flotabilidad de elementos sobre otros, su interaccion entre si y como afecta la particula flotante al perturbar las particulas que permiten la flotabilidad.

   Walker sobre perlin noise 3D El Concepto de la obra generativa es el de la simulacion de la marea y un barco flotando sobre el mar interactuando sobre ella, voy a usar la aleatoriedad de ruido perlin para afectar como se ve esa marea, la aleatoriedad para afectar el tono de color de la marea segun su altura (pensando en la altura de cada vertice) y con el mouse voy a afectar con la distribucion de probabilidad que tan intenso

   Verificacion de cumplimiento de requisitos, almenos 3 conceptos estudiados combinados:
   
- [x] Aleatoriedad
- [ ] Ruido perlin
- [ ] Random walker
- [ ] 3 conceptos combinados
- [ ] Coherencia y creatividad
- [ ] Interactiva y generativa en tiempo real
- [ ] Elemento fisico interactivo (mouse o teclado)


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

```
</details>

3. [Enlace al codigo en p5js sobre la obra generativa: titulo por definir](https://editor.p5js.org/DanielZafiro/sketches/C0liHXVGM)

4. Visualizacion de la obra (Giff)

<img src="" width="400">

</details>

<img src="https://github.com/user-attachments/assets/7e23ceba-cf53-4740-b2fd-e04366bbe522" width="400">

<details>
  <summary>Concepto obra generativa 2 Pecera</summary>

Inspiracion basada en el comportamiento de los cardumen de peces cuando se estan alimentando y cuando tienen cerca un depredador o un pez mas grande.

   Un conjunto de peces conforman el cardumen que actua como un perlin noise walker, hay un pez mas grande que podria ser un depredador, una tortuga, un delfin, si el pez grande se acerca al cardumen estos peces van a tener un comportamiento de evasion como se van a empezar a mover en una direccion aleatoria alejandose del pez grande, para que sus movimientos parezcan organicos usar el noise 2D de perlin funciona excelente por la suavidad del movimiento.

   además con el click del mouse se les pondrá alimento, tambien el alimento cae de manera aleatoria usando la distribucion no uniforme, la idea seria que los peces se acercaran a la particula mas cercana para alimentarse y aumentar su tamaño (radio) y alejarse del pez grande y que el pez grande fuera a cazar al pez mas grande con flight Levy.

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

```
</details>

3. [Enlace al codigo en p5js sobre la obra generativa: titulo por definir](https://editor.p5js.org/DanielZafiro/sketches/6Q7fMKdSM)

4. Visualizacion de la obra (Giff)


</details>

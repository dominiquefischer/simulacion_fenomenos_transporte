# Modelamiento de la dispersión de un contaminante en un río chileno (Río Maipo)
Este repositorio contiene el código, datos y figuras del proyecto "Modelamiento de fenómenos de transporte para Chile", en este caso de la dispersión de un contaminante en un río. Siendo un modelo 2D y estacionario.
## Descripción del proyecto
El proyecto desarrolla un modelo matemático y computacional para analizar la dispersión de contaminantes en ríos chilenos, específicamente en la cuenca del río Maipo, una de las más relevantes del país por abastecer cerca del 80% del agua potable de la Región Metropolitana.

La problemática principal que aborda es la contaminación hídrica generada por descargas industriales, junto con la creciente presión sobre los ecosistemas y los desafíos derivados del SEIA y la permisología, que han ralentizado la inversión en Chile y dificultan la evaluación ambiental de nuevos proyectos.

La relevancia en el contexto chileno se relaciona con que el río Maipo fue declarado Zona Saturada por ocho contaminantes, superando normas por más de tres años consecutivos, lo que refleja una crisis ambiental que amenaza tanto el consumo humano como la biodiversidad. A la vez, la incertidumbre técnica en la evaluación de impactos ambientales retrasa permisos, afectando la inversión y el crecimiento del país.

El proyecto se vincula directamente con los desafíos de sustentabilidad al proponer un modelo que permite predecir de manera rigurosa la dispersión de contaminantes, evaluar escenarios y justificar normativas de dilución. Esto contribuye a mejorar la gestión de cuencas, apoyar decisiones basadas en evidencia y acelerar procesos de evaluación ambiental sin relajar estándares, ayudando a equilibrar desarrollo económico y protección de los recursos hídricos, uno de los pilares de la sustentabilidad en Chile.


## Sistema físico modelado
El sistema corresponde a un río de **geometría rectangular**, con un ancho transversal $W$, largo ($L_x$) y profundidad $p(x)$. En la orilla se ubica una industria que descarga de manera continua un contaminante a través de un difusor horizontal ubicado entre ($y = h$) y ($y = h + W_d$), como ocurre en el caso del vertimiento de GRASCO S.A. en el río Maipo.
Se adjunta una imagen del sistema.
![Geometría](Imagenmodelo1.jpg)

Este tipo de descarga genera una **pluma de contaminante** que se desplaza y se mezcla con el agua del río a lo largo del eje longitudinal (x) y transversal (y) del cauce.

Componentes principales del sistema:
- El río, representado como un canal bidimensional (x–y), ancho y de profundidad moderada. Presenta un perfil de velocidad que varía espacialmente, siendo mayor en el centro del cauce y menor en las orillas debido al rozamiento.
  
    El río posee:
  - Velocidad longitudinal $u(x,y)$
  - Velocidad transversal $v(x,y)$ (cero en nuestro modelo)
  - Mezcla turbulenta lateral caracterizada por un coeficiente de difusión ε<sub>y</sub>

- El difusor o punto de descarga, ubicado en una posición lateral del río, descargando un caudal continuo de efluente con concentración conocida. Su ubicación respecto a la orilla determina la forma inicial de la pluma.

- El contaminante, sin reacción química compleja. Y su concentración evoluciona debido al transporte de masa y al decaimiento químico.

- El flujo del río, que controla la advección en la dirección longitudinal. La turbulencia natural del cauce induce un proceso de difusión turbulenta transversal.

**La formulación física** está gobernado por procesos de transporte de masa, específicamente:

1. Advección: El contaminante es arrastrado por la velocidad del río $u(x,y)$, lo que genera el movimiento predominante en el eje longitudinal. Consideramos $v(x,y) = 0$ al implementar el modelo.
2. Difusión turbulenta lateral: Describe la mezcla del contaminante en el eje transversal $y$ debido a la turbulencia del flujo. Está representada mediante un coeficiente ε<sub>y</sub>
3. Decaimiento químico: Se incorpora mediante un término de desaparición proporcional a la concentración, con constante $k_e$.
   
Estos mecanismos se acoplan para formar la ecuación diferencial parcial bidimensional utilizada en el modelo:
![Ecuación](Ecuacion.png)

donde:

- $C(x,y)$: concentración [M/L³]  
- $u(x,y)$: velocidad longitudinal [L/T]  
- $v(x,y)$: velocidad transversal [L/T]  
- ε<sub>y</sub>: difusividad turbulenta lateral [L²/T]  
- $k_e$: coeficiente de decaimiento [1/T]

Las **condiciones de borde** son las siguientes:

1. Concentración en la entrada (x = 0, y ):
   
    $C(0,y) = C0$    si   $h <= y <= h + Wd$
    
    $C(0,y) = 0$     en otro caso

2. En las "paredes" del río no hay flujo de contaminante hacia afuera del dominio. En $y = 0$ y $y = W$
   
   $dC/dy = 0$

Los **supuestos** adoptados en este modelo son los siguientes:
1.	Estado Estacionario
2.	Modelo horizontal bidimensional
3.	Difusión longitudinal despreciable
4.	Fluido Newtoniano
5.	Mezcla Binaria
6.	El flujo promedio no cambia con el tiempo
7.	El río presenta leves variaciones en su profundidad en la longitudinal.

## Descprición del método númerico utilizado
En este problema se resuelve la concentración estacionaria $C(x,y)$ mediante la discretización de diferencias finitas sobre una malla rectangular y con el método iterativo SOR (Successive Over-Relaxation).

El **método de diferencias finitas** se basa en aproximar las derivadas por cocientes de diferencias evaluados en puntos de una malla. Cada derivada se reemplaza por una combinación lineal de valores de la función en nodos vecinos. De este modo, una ecuación diferencial parcial continua se transforma en un sistema lineal algebraico que aproxima la solución en un conjunto discreto de puntos.

El uso de **SOR** es apropiado porque la ecuación discretizada es de tipo elíptico (difusión con advección estacionaria), lo que genera un sistema lineal grande pero bien condicionado para métodos iterativos. Además, SOR permite resolver el problema sin ensamblar explícitamente la matriz completa, trabajando nodo a nodo, lo que simplifica la implementación y reduce el consumo de memoria.

### Pasos para la discretización
(1) Se definió un dominio rectangular en las direcciones longitudinal y trannsversal con una malla uniforme:

$x_i$ $= i△x$, $i = 0,1,...,N_x$

$y_j$ $= j△y$, $j = 0,1,...,N_y$

Y en cada nodo se define: $C_{ij}$, $u_{ij}$ y $v_{ij}$

(2) Se aproximaron las derivadas:

















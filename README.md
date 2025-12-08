# Modelamiento de la dispersión de un contaminante en un río chileno (Río Maipo)
Este repositorio contiene el código, datos y figuras del proyecto "Modelamiento de fenómenos de transporte para Chile", en este caso de la dispersión de un contaminante en un río. Siendo un modelo 2D y estacionario.
## Descripción del proyecto
Se modela la dispersión de un contaminante puntual en un río, resolviendo la ecuación de advección-difusión con reacción en 2D (x - longitudinal, y - transversal). El principal objetivo es estimar como se diluye un contaminante a lo largo del río, y a que distancia alcana concentraciones bajas aceptables.
El dominio es representado como un canal rectangular de largo ​y ancho W, donde se resuelve la concentración estacionaria $C(x,y)$ mediante diferencias finitas y el método iterativo SOR (Successive Over-Relaxation).
___CATA

## Sistema físico modelado
El sistema corresponde a un río de **geometría rectangular**, con un ancho transversal $W$, largo ($L_x$) y profundidad $p(x)$. En la orilla se ubica una industria que descarga de manera continua un contaminante a través de un difusor horizontal ubicado entre ($y = h$) y ($y = h + W_d$). Siendo Wd el largo del difusor.
Se adjunta una imagen del sistema.
![Geometría](Imagenmodelo1.jpg)

El regimén es en estado estacionario y se asume que el río no tiene variaciones topográficas considerables y tiene un flujo estable.
El río posee:
- Velocidad longitudinal $u(x,y)$
- Velocidad transversal $v(x,y)$ (cero en nuestro modelo)
- Mezcla turbulenta lateral caracterizada por un coeficiente de difusión ε<sub>y</sub>

**La formulación física** considera un término de convección y de difusión, a continuación la ecuación:
![Ecuación](Ecuación.png)

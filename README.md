# Proyecto-Final-22955-F1

# 1.1 Descripción del problema: 

En situaciones de emergencia, como huracanes o inundaciones, la evacuación oportuna y efectiva es crucial para salvaguardar vidas. En el estado de Florida (Estados Unidos), las carreteras principales juegan un papel vital en trasladar a las personas desde zonas de riesgo hacia áreas seguras. Sin embargo, durante una evacuación inmediata, el flujo vehicular combinado de evacuados y residentes de zonas con menor riesgo de ser afectadas, aumenta considerablemente, llegando incluso al punto de sobrepasar el volumen de tráfico permitido por autopistas y generar colapso. Este proyecto busca abordar este desafío mediante el cálculo del flujo máximo que pueden soportar estas rutas clave. Con base en datos de tráfico ajustados, la herramienta permite identificar cuellos de botella y proporciona información valiosa para que los responsables de la evacuación optimicen la distribución de vehículos, asegurando así una salida más eficiente y reduciendo el riesgo de obstrucciones críticas.

![Texto Alt](map.jpg)

## ¿Por qué escogimos esta zona? 

Escogimos Florida, específicamente el condado de Collier, debido a su alta vulnerabilidad a desastres naturales, especialmente huracanes, que obligan a realizar evacuaciones de carácter rápido y masivo de zonas costeras y urbanas.
Teniendo en cuenta que la infraestructura en estas vías es variable y su capacidad vehicular no es la misma a lo largo del condado de Collier, la gestión del flujo vehicular en una evacuación repentina, se convierte en un desafío logístico, dando así lugar a la importancia de contar con un análisis preciso de las rutas y la capacidad de las mismas para soportar el flujo vehicular al que se enfrentarán.
Para cumplir con los requisitos del proyecto, se decidió convertir los nodos de salida y de entrada de la evacuación en variables que el usuario deberá elegir, y de esta manera mostrar la dinámica del grafo y del algoritmo.


## ¿Qué tipo de grafo modela el problema? 

Grafo dirigido, ponderado, de 24 nodos, con origen y sumidero dinámico. Cada nodo representa puntos clave de flujo vehicular y cada arista basa su capacidad en el 10% del Tránsito Promedio Diario Anual (TPDA) de la vía que representa. El cálculo del 10% del TPDA para una vía, es una aproximación realista a la cantidad de vehículos que circulan en ella durante un estado de congestión masiva.

## Parámetros clave

Este análisis considera el peor escenario: una evacuación urgente sin tiempo para planificar. Evaluando las capacidades de las vías, podemos determinar la máxima cantidad de vehículos que pueden circular, lo que ayudará a organizar mejor la evacuación y evitar embotellamientos mayores.
   
A partir de lo mencionado, es necesario tener presentes ciertos parámetros clave, ya que son fundamentales tanto para el análisis y comprensión del problema como para su correcta implementación en código.

* Rango de Tiempo:
El análisis se realiza en el rango de una hora, específicamente en la hora de mayor congestión, cuando el tráfico está en su máxima capacidad.
* Vías principales:
La evacuación se considerará en base a las vías principales designadas por el departamento de Transporte del estado Florida, pues pese a que hay muchas vías que podrían considerarse, las seleccionadas son las más aptas para un flujo de tal magnitud.
* Enfoque en estado de congestión masiva:
Se realiza el enfoque durante este momento, ya que para optimizar la evacuación es necesario tener presente la condición más esperada para una operación como esta.

# 1.2 Investigación del problema y alternativas de solución:

## Introducción al problema de flujo máximo

Así como es posible modelar un trayecto en un grafo dirigido para calcular el camino más corto de un punto a otro, también podemos interpretar el grafo como una red de flujo donde cada arista tiene una capacidad máxima, y los nodos fuente y sumidero, con grado de salida mayor a cero y cero respectivamente, establecen el inicio y llegada del flujo que transporta el grafo. 
Lo interesante de este problema y debido a lo cual se asocia con el problema anterior, es la idea de la representación de cada arista como un conducto por el que fluye algo en específico, volviendo de este problema un modelo para diversos campos, como lo son el del paso de fluidos por conductos, distribución óptima de una red eléctrica, circulación de información a través de redes de comunicación, y flujo vehicular.

En el problema de flujo máximo se busca calcular la mayor tasa a la que podemos transportar el material desde la fuente hasta el sumidero sin violar ninguna de las restricciones de capacidad, o en términos relacionados con el problema establecido, la máxima cantidad de vehículos que pueden recorrer un condado en estado de evacuación inmediata sin sobrepasar el límite de las vías y generar embotellamientos de mayor magnitud.

## Nodos y Aristas

Además de establecer qué representan los nodos y aristas en el problema, nos valemos de mencionar algunos atributos clave de estos elementos.
Es importante aclarar que para la correcta implementación del método, cada arista (u, v) o arista positiva al flujo, que pasa a través del grafo G = (V, E), debe tener una capacidad no negativa, además de no tener otra arista (v, u), o arista opuesta al flujo, ya que esta arista se reserva para el manejo de la red residual y almacenar los valores que esta misma red genera dentro del camino aumentante.

## Algoritmos de exploración de grafos

El método Ford-Fulkerson funciona bajo DFS (Depth First Search) o Búsqueda en Profundidad, lo que lo vuelve propenso a tiempos de ejecución indeterminados. Una solución a este problema es la implementación de BFS (Breadth First Search) o Búsqueda en anchura, la cual funciona bajo el algoritmo de Edmonds Karp dentro del método de Ford-Fulkerson, convirtiéndolo así en un problema con tiempo de ejecución polinomial.

## Evaluación y modelado de una red residual

El corazón del método de Ford-Fulkerson está en el funcionamiento de la red residual , la cual además de establecer nuevas capacidades para todas las aristas opuestas y positivas al flujo, restringe la búsqueda del camino (BFS) por aristas positivas al flujo,  donde la capacidad actual de la arista sea igual a cero. 
 
La red residual funciona de la siguiente manera: 

1. Se establece el camino por el cual se va a recorrer el grafo en la siguiente iteración.
2. Se almacena el flujo mínimo de este camino, es decir, el valor más pequeño entre las aristas que lo conectan.
3. La red residual se empieza a crear a partir de las siguientes condiciones para aristas opuestas y positivas al flujo:
4. A cada arista (u, v) o arista positiva al flujo se le resta el valor del flujo mínimo calculado previamente.
5. A cada arista (v, u) o arista opuesta al flujo se le suma el valor del flujo mínimo calculado previamente. 

Como consecuencia a la red residual creada, las aristas positivas al flujo son propensas a quedar con un valor de cero, tanto en la primera como en las futuras iteraciones sobre el grafo. Estos nuevos valores sobre el grafo, forman un conjunto total de nuevas aristas, también conocido como camino aumentante.

## Camino aumentante

La consecuencia de cada iteración de búsqueda del BFS y la respuesta de la red residual, es un camino aumentante que establece nuevos valores para cada arista, tanto opuesta al flujo como positiva al flujo. Es por esto que el grafo base, el cual empieza sin aristas opuestas al flujo, deja de ser iterado a partir de la segunda búsqueda con BFS, para ser reemplazado por uno con caminos aumentantes nuevos en cada iteración.

# Resultados y Conclusiones

El flujo final calculado por el código nos muestra las rutas a tener en cuenta para obtener el flujo vehícular máximo dentro del condado de Collier, logrando así una evacuación aún más efectiva hacia Immokalee, con menos probabilidades de saturar y comprometer vías alternas.

# README - PRACTICA 2 - VISIÓN POR COMPUTADOR
### Jaime Cuenca Relinque y Daniel Hernández Sánchez
#### **Objetivo del informe**

En este documento se presenta la memoria de la segunda práctica de la asignatura, centrada en las técnicas esenciales del procesamiento digital de imágenes. A lo largo del trabajo se abordan conceptos clave como la conversión de espacios de color, la manipulación de matrices de píxeles, el análisis de histogramas y la segmentación espacial.

El objetivo principal es comparar la eficacia de los detectores de bordes Canny y Sobel mediante el estudio de la densidad de píxeles. Como cierre, se propone un prototipo interactivo en tiempo real que aplica estos principios de visión por computador a un entorno práctico.

### 2.1 - Tarea 1

#### ¿Qué teníamos que conseguir?

En el ejemplo anterior del cuaderno nos enseñaron a contar los píxeles de borde por columnas. El objetivo ahora es encontrar la fila que concentra el mayor numero de bordes verticales. En esta tarea nos pedían hacer lo opuesto: analizar la imagen del mandril por filas.

Los pasos a seguir eran:

Sumar cuántos píxeles blancos (los bordes que detecta Canny) hay en cada fila.

Buscar cuál es el número máximo de píxeles de borde en una sola fila (maxfil).

Filtrar y mostrar las filas que tuvieran al menos el 90% de ese máximo.

Dibujar esas filas sobre la propia imagen de Canny para ver visualmente dónde se concentran.

#### ¿Cómo lo hemos hecho?

Nos basamos en la lógica del código de las columnas, pero cambiando la dirección del análisis de vertical a horizontal:

Sumar por filas: En lugar de cv2.reduce(..., 0, ...) (que suma hacia abajo), usamos dimension = 1 para sumar a lo largo de cada fila.

Contar píxeles reales: Como los píxeles de borde en Canny valen 255, dividimos el resultado entre 255 para saber el número exacto de píxeles blancos.

Calcular porcentajes y filtrar: Dividimos entre el ancho total de la imagen para saber qué porcentaje de la fila es borde. Después buscamos el pico máximo (maxfil) y nos quedamos con las filas que superan el 0.90 * maxfil.

Marcar en la imagen: Con la función plt.hlines() dibujamos líneas verdes horizontales sobre el mapa de Canny en esas posiciones clave.

Los comentarios sobre que hace cada línea de código se encuentra en el cuaderno a la derecha de cada línea.

#### ¿Qué resultados hemos obtenido?

Al ejecutar el código sobre la imagen del mandril, estos han sido los datos:

Fila con más bordes: La fila 12, que tiene 220 píxeles blancos (casi el 43% de la fila es borde).

Filas destacadas: En total han salido 7 filas que cumplen la condición del 90% (tienen unos 200 píxeles o más): las filas 6, 12, 15, 20, 21, 88 y 100.

#### Imágenes adjuntando los resultados obtenidos

<img width="525" height="215" alt="image" src="https://github.com/user-attachments/assets/6f17efc4-6399-4db0-b463-9da4cad53239" />

<img width="635" height="467" alt="image" src="https://github.com/user-attachments/assets/f34b2b83-f25d-4821-a87b-efd776e142fa" />

También podemos probar con otra foto para ver que resultados se obtienen. En este caso he cogido el logo de la ULPGC que tiene los bordes verticales de las letras, una linea vertical y un dibujo de infoDOC. Adjunto en la siguiente imagen los resultados

<img width="636" height="137" alt="image" src="https://github.com/user-attachments/assets/886f1861-119d-41e6-983f-9ba8ea57e4ce" />

<img width="628" height="471" alt="image" src="https://github.com/user-attachments/assets/5232651d-d9eb-4f53-b096-e82a70a795d0" />

Como podemos observar los picos son las filas en las que las letras están "posadas" (las filas en las que están las partes superiores e inferiores de las letras)

### 2.2 - Tarea 2

#### ¿Qué teníamos que conseguir?

En el caso de la tarea 2, se plantea un análisis de bordes tanto verticales como horizontales utilizando Canny y Sobel.

El análisis con cada detector de bordes consiste en:
Calcular el valor máximo de la cuenta por filas y columnas.
Mostrar las filas y columnas por encima del 0.90*máximo. 
Visualiza los resultados obtenidos con gráfica y en la imagen utilizada. 

Además de la pregunta: ¿Cómo se comparan los resultados obtenidos a partir de Sobel y Canny?

#### ¿Cómo lo hemos hecho?

Para reducir el ruido presente en la imagen comenzamos aplicando un filtro Gaussiano, lo cual mejora la estabilidad de los detectores de bordes al eliminar pequeñas variaciones de intensidad.

Una vez utilizado el filtro sobre la imagen, aplicamos el operador Sobel en las direcciones horizontal y vertical para obtener los bordes en cada direccion.

Debido a que la salida de Sobel contiene distintos niveles de intensidad, aplicamos un umbral con valor 130. Los píxeles con una intensidad superior a dicho valor se transforman en 255 (blanco) y el resto en 0 (negro). De esta forma obtenemos una imagen mas equivalente a la salida de Canny, lo cual facilitará su comparación.

A continuación ya podemos realizar el análisis por columnas y filas. Para ello utilizamos de nuevo función cv2.reduce() para sumar los valores tanto por columnas (0) como por filas (1).

Como los píxeles de borde tienen valor 255, dividimos posteriormente entre este valor para obtener el número real de píxeles de borde detectados. Además de realizar todos los cálculos pertinentes: máximos por columna/fila, 0.90maximo por columna/fila y sus respectivas posiciones.

Por otro lado, el mismo análisis por columna y fila es repetido utilizando la salida del detector Canny.

Finalmente, utilizamos las funciones plt.vlines() y plt.hlines() para remarcar visualmente las columnas y filas más significativas sobre la imagen del mandril.

#### ¿Qué resultados hemos obtenido?

Análisis con Sobel tras umbralizar:

Columna con mas bordes: 104 con un 42% de sus pixeles siendo borde
Columnas Destacadas: 104, 105, 127, 228 

Fila con mas bordes: 4 y 24 ambas con un 48%
Filas Destacadas: Se han detectado 18 filas con >= 0.90*max

<img width="543" height="485" alt="image" src="https://github.com/user-attachments/assets/c5f5114d-bec5-4cb1-903a-04f8aa584ee3" />

Análisis con Canny:

Columna con mas bordes: 104 con un 26%
Columnas Destacadas: 92, 104, 119

Fila con mas bordes: 24 con un 35%
Filas Destacadas: 6, 8, 20, 24, 100.

<img width="527" height="224" alt="image" src="https://github.com/user-attachments/assets/c274b520-6560-4fb6-a6ae-ffaa559b623c" />

#### Imágenes adjuntando los resultados obtenidos

<img width="592" height="469" alt="image" src="https://github.com/user-attachments/assets/08e0521e-abf0-409e-9fa8-17dc2f0941e6" />

<img width="594" height="468" alt="image" src="https://github.com/user-attachments/assets/7a98183e-e921-4756-ada5-6b16de01f496" />

Tras realizar el análisis de ambas técnicas, se observa que Sobel detecta una mayor cantidad de bordes que Canny. Esto se aprecia tanto en el porcentaje de píxeles detectados como en el número de filas y columnas destacadas, donde Sobel identifica 18 filas por encima del 90 % del máximo frente a únicamente 5 en el caso de Canny.

Para entender mejor estos resultados hay que entender mejor como funciona cada detector. Por un lado, Sobel ofrece una detección más abundante, ya que genera respuesta en cualquier zona donde exista un cambio apreciable de luminosidad, de ahí que sea necesario el umbralizado. Por otro lado, Canny proporciona menor cantidad de datos al incluir etapas adicionales que permiten eliminar respuestas redundantes y conservar únicamente los contornos más significativos, obteniendo una detección más limpia y precisa.

<img width="515" height="472" alt="image" src="https://github.com/user-attachments/assets/f8e29c9e-27a2-40c6-becf-ab2a1d01dea9" />

### 2.3 - Tarea 3

#### ¿Qué teníamos que conseguir?

Por último, teníamos que proponer un demostrador reinterpretando la parte de procesamiento de la imagen, en nuestro caso utilizamos el código de ejemplo de diferencia de imágenes como punto inicial para realizar la propuesta. En nuestro caso, vamos a utilizar la diferencia de imágenes y un detector de bordes para realizar un filtro que dibuje encima de la cabeza un OVNI. 

#### ¿Cómo lo hemos hecho?

Para comenzar, se calcula la diferencia absoluta entre el fotograma actual y el anterior utilizando la función cv2.absdiff(). Lo cual tiene como resultado únicamente las zonas que han cambiado entre ambos frames.
Posteriormente la imagen de diferencias se convierte a escala de grises y se le aplica un filtro Gaussiano para eliminar pequeñas variaciones producidas por ruido o fluctuaciones de iluminación.

Una vez suavizada la imagen, se emplea el operador Sobel en el eje X (En este caso únicamente se utiliza Sobel-X porque el objetivo no es detectar todos los bordes de la imagen, sino localizar las posiciones horizontales donde existe movimiento.) 

Después se aplica un umbral binario para conservar únicamente las respuestas más significativas. Con la imagen ya binarizada se realiza un análisis por columnas similar al desarrollado en tareas anteriores. Para ello se suman los valores de cada columna mediante cv2.reduce(). 

Calculando el número de píxeles por columna se seleccionan aquellas columnas que presentan más de 12 píxeles activos. Estas columnas corresponden a las posiciones donde existe una concentración significativa de bordes y, por tanto, movimiento.

A partir de la primera y última columna detectadas se calcula la anchura completa de la región activa y su punto central.

Finalmente se utiliza este punto para posicionar la imagen del OVNI sobre el vídeo. Para evitar copiar el fondo blanco asociado a la imagen, se genera una máscara que únicamente conserva los píxeles pertenecientes a la nave. De esta forma el OVNI aparece integrado en la escena y sigue automáticamente la posición de las personas u objetos que se desplazan delante de la cámara.

#### ¿Qué resultados hemos obtenido?

Como se puede observar en los resultados el OVNI es dibujado encima de la cabeza y sigue el movimiento de la misma. 

#### Imágenes adjuntando los resultados obtenidos

![Video OVNI prueba](./ovni_demo1.gif)

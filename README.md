# README - PRACTICA 2 - VISIÓN POR COMPUTADOR
### Jaime Cuenca Relinque y Daniel Hernández Sánchez
#### **Objetivo del informe**

En este documento se presenta la memoria de la práctica VC_P1.ipynb, centrada en las técnicas esenciales del procesamiento digital de imágenes. A lo largo del trabajo se abordan conceptos clave como la conversión de espacios de color, la manipulación de matrices de píxeles, el análisis de histogramas y la segmentación espacial.

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

![image.png](attachment:f1c2054d-d048-4e8c-a87c-6969f1d61eb6.png)

También podemos probar con otra foto para ver que resultados se obtienen. En este caso he cogido el logo de la ULPGC que tiene los bordes verticales de las letras, una linea vertical y un dibujo de infoDOC. Adjunto en la siguiente imagen los resultados

Como podemos observar los picos son las filas en las que las letras están "posadas" (las filas en las que están las partes superiores e inferiores de las letras)

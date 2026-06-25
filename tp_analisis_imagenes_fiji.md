# Trabajo Práctico: Análisis de Bioimágenes

**Concepto Fundamental:** **Una imagen es una matriz de números**, y lo que vemos en la pantalla es simplemente una representación visual de esos datos.

**Objetivo:** Explorar la naturaleza de las imágenes digitales, aplicar operaciones matemáticas y filtros de procesamiento, gestionar Regiones de Interés (ROIs) y preparar figuras multicanal con estándares de publicación científica.

**Materiales necesarios:** 
- * Una imagen monocromática de una grilla de calibración.
- * Un set de imágenes correspondientes a distintos canales fluorescentes del mismo campo óptico.

---

### La Naturaleza Digital de la Imagen y Representación Visual

1. **Exploración de píxeles:** Abre tu imagen de la grilla de calibración en FIJI (`File > Open...`). Pasa el cursor sobre distintas zonas de la imagen y observa la barra de estado de FIJI (debajo de las herramientas). Notarás las coordenadas (x, y) y el valor numérico (intensidad) de cada píxel. Haz mucho zoom hasta notar los cuadrados correspondientes a cada píxel.
2. **Perfil de intensidad:** Traza una línea recta cruzando algunas líneas de la grilla y genera un gráfico de perfil usando `Analyze > Plot Profile` (o `Ctrl+K` / `Cmd+K`). Observa los valores.
3. **Manipulación de la representación:** * Cambia la Tabla de Búsqueda de Color (LUT) yendo a `Image > Lookup Tables` y elige una distinta (ej. *Fire* o *HiLo*).
   * Modifica el Brillo y Contraste desde `Image > Adjust > Brightness/Contrast...`.
4. Discute con los demás qué implica que la imagen sea 8-bit, 16-bit o 32-bit.
4. **Re-evaluación:** Con los nuevos ajustes visuales aplicados, repite el paso 1 (pasar el cursor) y el paso 2 (trazar la misma línea y ver el *Plot Profile*).

> **💡 Pro-Tip sobre la visualización:**
> Al comparar los resultados del paso 2 y el paso 4, descubrirás uno de los conceptos más importantes en análisis de imágenes: cambiar el Brillo/Contraste o la LUT altera cómo *tus ojos* ven la imagen en el monitor, pero **no modifica los valores numéricos reales** de la matriz subyacente. El perfil trazado en el gráfico será exactamente el mismo.

5. **Calibra la escala:** Ve a `Analyze > Set Scale...`.
6. **Ingresa los datos:**
   * En la casilla **Distance in pixels**, FIJI ya habrá colocado la longitud de la línea que acabas de trazar.
   * En **Known distance**, ingresa la suma total de las dimensiones físicas (en µm) de los agujeros y barras que abarcaste con tu línea.
   * En **Pixel aspect ratio**, déjalo en `1.0` (asumimos píxeles cuadrados).
   * En **Unit of length**, escribe `um`.

> * **Datos del patrón (Grilla TEM):** Los agujeros de la grilla miden **132 µm** de lado y las barras (el espacio sólido entre agujeros) miden **33 µm** de ancho.
> * **Datos del hardware:** 
>    * Objetivos utilizados: 10x (NA: 0,3) y 60x (NA: 1,25).
>    * Especificaciones de la cámara: El tamaño de píxel físico en el sensor es de **6,45 µm**, y la resolución del sensor es de 1392 x 1040 píxeles.

7. **Registra el tamaño de píxel:** Observa el texto en la parte inferior de esa misma ventana que dice `Scale: X pixels/um`. Anota la inversa de ese valor (1 / X), o simplemente fíjate en la barra inferior de la ventana principal de tu imagen donde FIJI te dirá cuánto mide 1 píxel (ej. `1 pixel = 0.645x0.645 um`).

> **💡 Pro-Tip sobre la calibración:**
> Al analizar el perfil de intensidad de la grilla podemos ver cuantos píxeles entran en el espacio definido entre las barras de la grilla. Al dividir la cantidad de píxeles por el espacio definido podemos obtener el tamaño de cada píxel. ¿Es mejor hacer estadística de varias barras o tomar varias barras en una sola línea? ¿Se condice con lo esperado teóricamente?

8. **Comparemos ese valor con el estimado teóricamente**: En este método, no usaremos imágenes, sino la física y óptica del sistema. El tamaño del píxel proyectado en la muestra depende directamente de qué tan grande es el píxel real dentro de la cámara y de cuánto fue ampliada la imagen por el objetivo de microscopía antes de llegar al sensor.

    8.1. Utiliza la siguiente fórmula para calcular teóricamente el tamaño del píxel:

    $$Tamaño\_Píxel = \frac{Tamaño\_Físico\_Sensor}{Magnificación\_Objetivo}$$

    *(Nota: Esta fórmula asume que el adaptador de la cámara o "C-mount" tiene una magnificación de 1x. Si tuviera un lente interno de 0.5x o 2x, habría que incluirlo en el denominador multiplicando a la magnificación del objetivo).*

    8.2. Calcula el tamaño de píxel para el objetivo de **10x** y para el de **60x** sabiendo que el *Tamaño Físico del Sensor* es de **6,45 µm**.

---

### Operaciones Matemáticas y Filtros

Utilizando cualquier imagen monocromática, exploraremos cómo alterar la matriz de datos.

1. **Aritmética de Escalares:** Carga de nuevo la imagen original. Ve a `Process > Math`. Experimenta aplicando operaciones de suma (`Add`), resta (`Subtract`), multiplicación (`Multiply`) y división (`Divide`) por un valor escalar constante. Observa cómo cambia la imagen.
2. **Aritmética entre Imágenes:** Abre dos imágenes distintas (o la imagen original y su máscara). Ve a `Process > Image Calculator...`. Realiza operaciones matemáticas entre ambas matrices de imágenes (ej. sumar una a la otra o multiplicarlas).
3. **Suavizado (Filtros Espaciales):** Aplica un filtro para difuminar el ruido yendo a `Process > Filters > Gaussian Blur...`.
4. **Detección de Bordes:** Usa el filtro `Process > Find Edges` para resaltar los gradientes de intensidad de tu imagen.

---

### Regiones de Interés (ROIs) y Mediciones

1. **Máscara y Umbralización:** Ve a `Image > Adjust > Threshold...` y genera una máscara binaria. Explora los diferentes métodos para determinar el umbral utilizando la estadística de intensidad de píxeles. Haz clic en *Apply*.
2. **Uso del ROI Manager:** Ve a `Analyze > Analyze Particles...`. Esto nos permitirá crear regiones de interés por cada objeto detectado por separado.
3. **Medición de Propiedades:** Primero ve al menu de `Analyze > Set Measurements...` y explora las opciones de parámetros a cuantificar. En el *ROI Manager*, asegúrate de deseleccionar todas las ROIs de la lista y haz clic en **Measure**. Esto medirá las propiedades (área, intensidad, etc.) de todas tus regiones guardadas simultáneamente sobre la imagen activa. *Nota: No olvides utilizar la escala calculada previamente mediante `Analyze > Set Scale...`.

---

### Composición Multicanal (Color)

1. **Carga de canales:** Abre las imágenes individuales que corresponden a los diferentes canales fluorescentes de tu muestra.
2. **Combinación a color:** Ve a `Image > Color > Merge Channels...`. Asigna el color apropiado a cada imagen (por ejemplo, rojo (C1), verde (C2), azul (C3)) y haz clic en *OK*. Se generará una imagen *Composite*.
3. **Ajuste independiente:** Con la imagen color seleccionada, abre el panel `Image > Adjust > Brightness/Contrast...`. Usa el control deslizante de la parte inferior de la ventana principal de la imagen para moverte entre los canales y ajusta la LUT, el brillo y el contraste de cada uno por separado para lograr el balance visual deseado.

---

### Preparación de Imágenes para Publicación

Para que una imagen tenga validez científica en una publicación, debe incluir referencias espaciales e información de escala.

1. **Calibración espacial:** Asegúrate de que la imagen esté calibrada (`Analyze > Set Scale...`).
2. **Barra de Escala:** Ve a `Analyze > Tools > Scale Bar...`. Configura el ancho, color y posición de la barra de escala para que sea claramente visible frente al fondo.
3. **Barra de Calibración:** (Útil para mostrar mapeos de intensidades o LUTs térmicas). Ve a `Analyze > Tools > Calibration Bar...` y ajusta los parámetros.
4. **Creación de Montaje:** A partir de tu imagen multicanal de la Parte 4, ve a `Image > Stacks > Make Montage...`. Configura las filas y columnas para exportar una figura lista para publicación que muestre cada canal en escala de grises o color por separado, finalizando con la imagen combinada (*Merge*).

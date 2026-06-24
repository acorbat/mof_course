# Práctica: Estimación de Tamaño y Límite de Resolución en FIJI

**Objetivo:** Comprender la diferencia práctica entre **magnificación** (aumento) y **Apertura Numérica, NA** (resolución) mediante la medición del perfil de intensidad de filamentos de actina utilizando FIJI, y aprender a exportar datos crudos para su posterior análisis.

**Materiales necesarios:** Tres imágenes de la misma preparación de filamentos de actina adquiridas con diferentes configuraciones de objetivos:
* **Imagen A:** Objetivo 10x, NA 0.3
* **Imagen B:** Objetivo 60x, NA 0.65
* **Imagen C:** Objetivo 60x, NA 1.25

---

### Extracción del Perfil de Intensidad

Comencemos por la imagen correspondiente a la magnificación de 60x y NA de 0.65. Realizemos los siguientes pasos en FIJI:

1. **Abre la imagen:** Arrastra el archivo al panel principal de FIJI o ve a `File > Open...`
2. **Selecciona la herramienta de línea:** En la barra de herramientas, haz clic en *Straight Line*.
3. **Traza la línea de medición:** Haz zoom sobre un grupo de filamentos de actina que se encuentren separados entre si. Dibuja una línea perpendicular que los cruce, abarcando un poco de fondo a cada lado.

> **💡 Pro-Tip para mejorar la estimación:**
> Cuando trazas una línea diagonal, los píxeles no se alinean perfectamente con tu trazo, lo que obliga a FIJI a **interpolar** los valores de intensidad. Para obtener la medición más pura y exacta, busca unos filamentos que estén orientados vertical u horizontalmente y **dibuja tu línea de corte perfectamente paralela al eje X o al eje Y** (puedes mantener presionada la tecla `Shift` mientras dibujas para forzar la línea a 0°, 90°, etc.). De esta forma, FIJI leerá los valores crudos de los píxeles sin necesidad de interpolar.

4. **Genera el gráfico:** Ve a `Analyze > Plot Profile` (o usa el atajo `Ctrl + K` / `Cmd + K`).
5. **Mide el ancho del filamento en FIJI:** En la ventana del gráfico, observa la campana de Gauss que se forma. Estima el ancho del filamento midiendo la anchura de la campana a la mitad de su altura máxima. A esto se le conoce como **FWHM** (*Full Width at Half Maximum*).
6. **Registra tus datos manuales:** Anota el valor del FWHM en nanómetros (asegúrate de que la imagen esté calibrada espacialmente en `Analyze > Set Scale`).

---

### (Opcional): Exportación de Datos a CSV para Análisis Avanzado

Para realizar una comparación matemática más rigurosa, exportaremos los valores numéricos del perfil de intensidad de cada imagen:

1. En la ventana del gráfico generado (`Plot of...`), dirígete a la parte inferior y haz clic en el botón **`Data >>`**.
2. En el menú desplegable que aparece, selecciona la opción **`Save Data...`**.
3. Guarda el archivo con un nombre descriptivo que identifique la condición (por ejemplo, `perfil_actina_40x.csv`). Asegúrate de escribir la extensión `.csv` al final del nombre si el sistema no la añade automáticamente.
4. *Alternativa:* También puedes hacer clic en el botón **`List`** (se abrirá una tabla con dos columnas: *Distance* e *Intensity*), y luego ir a `File > Save As...` dentro de esa ventana de la tabla.

**Nota para el análisis posterior:** Repite este proceso de exportación para las otras imágenes. Con estos tres archivos `.csv`, podrás importar los datos en un software externo (como Excel, Origin, GraphPad o mediante un script de Python/R) para normalizar las intensidades y graficar los tres perfiles superpuestos en un mismo eje, facilitando la comparación visual de las curvas.

---

### Repetición y Recolección de Datos

Completa la siguiente tabla con tus resultados y observaciones individuales:

| Imagen | Magnificación | Apertura Numérica (NA) | Ancho del filamento (FWHM) estimado | Archivo CSV exportado |
| :--- | :--- | :--- | :--- | :--- |
| **A** | 10x | 0.30 | *[Tu valor aquí]* nm | `perfil_actina_10x_na030.csv` |
| **B** | 60x | 0.65 | *[Tu valor aquí]* nm | `perfil_actina_60x_na060.csv` |
| **C** | 60x | 1.25 | *[Tu valor aquí]* nm | `perfil_actina_60x_na125.csv` |

---

### Análisis y Conclusiones

Responde a las siguientes preguntas basándote en los datos recolectados:

1. **Comparación B vs. C:** Ambas imágenes tienen la misma magnificacion (60x), pero distinta apertura numérica (NA 0.65 vs NA 1.25). ¿Cómo son los anchos (FWHM) medidos entre ambas? ¿Cambia la resolución real de la estructura?
2. **Comparación A vs. B:** Ambas imágenes tienen aumentos diferentes, pero la Imagen B tiene una apertura numérica mucho mayor. ¿Cuál filamento se mide más "delgado" o definido en tus perfiles de intensidad?

#### Conclusión Clave
Un filamento de actina real mide aproximadamente **7 nm** de diámetro. Sin embargo, tus mediciones seguramente oscilan entre **100 nm y 400 nm**. Como el filamento es mucho más pequeño que el límite de difracción de la luz, lo que estás midiendo en realidad no es el grosor biológico de la actina, sino la respuesta del propio sistema óptico frente a un punto luminoso (su *Point Spread Function* o PSF). 

Por lo tanto, debes concluir que **el ancho detectado de una estructura sub-resolutiva depende exclusivamente de la Apertura Numérica (NA) del objetivo, y no de su magnificación.** Aumentar la "magnificacion" (por ejemplo pasar de 40x a 60x manteniendo el mismo NA de 0.6) solo hace que la mancha borrosa se distribuya en más píxeles de la cámara, pero no mejora la resolución ni hace que el filamento se detecte más estrecho.
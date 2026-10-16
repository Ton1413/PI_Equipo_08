# Taller TinyML: clasificación de botella y mouse

Clasificación de imágenes de **Botella** y **Mouse**, con evidencias de inferencia en una **XIAO ESP32S3** mediante Arduino IDE y un ejemplo de cámara de Edge Impulse.

## 1. Objetivo del proyecto

Reconocer dos tipos de objetos a partir de imágenes y observar las puntuaciones de confianza que entrega el modelo en el monitor serie. El taller relaciona la preparación del dataset, el modelo de aprendizaje automático y su ejecución en un microcontrolador.

## 2. Archivos del taller

Los **tres ZIP se encuentran en la carpeta principal del repositorio**, junto a este README. Las cinco imágenes de evidencia están dentro de `imagenes/`.

| Archivo | Contenido verificado | Uso |
|---|---|---|
| [botella_images_50.zip](botella_images_50.zip) | 50 imágenes JPG | Dataset correspondiente a Botella |
| [mouse_images_50.zip](mouse_images_50.zip) | 50 imágenes JPG | Dataset correspondiente a Mouse |
| [gg.zip](gg.zip) | Archivo `gg.lite` | Archivo de modelo entregado |


## 3. Materiales y herramientas

- Placa XIAO ESP32S3 y cámara para capturar las imágenes.
- Cable USB con un extremo tipo C, según las indicaciones del taller.
- Computadora con Arduino IDE; en las capturas se observa la versión 2.3.10.
- Ejemplo `esp32_camera.ino` de Edge Impulse.
- Dataset de dos clases y archivo de modelo `gg.lite`.

## 4. Dataset: imágenes y etiquetas

| Clase | Cantidad de imágenes | Formato | Resolución de las muestras revisadas |
|---|---:|---|---|
| Botella | 50 | JPG | 320 × 240 píxeles |
| Mouse | 50 | JPG | 320 × 240 píxeles |
| **Total** | **100** | JPG | — |

Las muestras revisadas de botella muestran una botella plástica en diferentes posiciones; las de mouse muestran un ratón de computadora sobre una mesa. La variación de posición permite aportar ejemplos visuales distintos de cada objeto.

### Imagen 1. Predicción de la clase Botella

![Monitor serie con predicción de botella](imagenes/02_prueba_botella.png)

Se observa Arduino IDE con la placa **XIAO_ESP32S3**, el archivo `esp32_camera.ino` y el monitor serie. Una lectura muestra:

| Clase | Puntuación | Porcentaje |
|---|---:|---:|
| Botella | 0.98828 | 98.828 % |
| Mouse | 0.01172 | 1.172 % |

La clase con mayor puntuación es **Botella**. En otra lectura visible, Botella obtiene 0.97266, equivalente a 97.266 %. El programa informa **4 ms de DSP**, **170 ms de clasificación** y **0 ms de anomalía**.

El fragmento de código visible recorre los píxeles, reorganiza los canales de color de BGR a RGB y verifica que el modelo corresponda a un sensor de cámara.

### Imagen 2. Predicción de la clase Mouse

![Monitor serie con predicción de mouse](imagenes/03_prueba_mouse.png)

La misma interfaz muestra ahora una puntuación mayor para **Mouse**. Una de las lecturas es:

| Clase | Puntuación | Porcentaje |
|---|---:|---:|
| Botella | 0.00391 | 0.391 % |
| Mouse | 0.99609 | 99.609 % |

También se observan puntuaciones de 98.828 % y 97.266 % para Mouse. Esto evidencia que la salida del clasificador puede favorecer la segunda clase. Los tiempos informados permanecen en 4 ms de DSP y 170 ms de clasificación.

### Imagen 3. Lecturas adicionales de Botella

![Lecturas adicionales de botella en el monitor serie](imagenes/04_prueba_botella_adicional.png)

Se observa el encabezado del ejemplo de Edge Impulse y nuevas predicciones:

| Lectura visible | Botella | Mouse | Clase con mayor puntuación |
|---|---:|---:|---|
| 1 | 96.484 % | 3.516 % | Botella |
| 2 | 95.703 % | 4.297 % | Botella |
| 3 | 95.703 % | 4.297 % | Botella |

Estas lecturas mantienen Botella como clase dominante, aunque con puntuaciones menores que las de la imagen 2. La captura no permite establecer la causa de esa variación. La posición, el encuadre o la iluminación son variables que pueden estudiarse en pruebas posteriores.

### Imagen 4. Configuración de cámara y salida visible

![Ejemplo CameraWebServer1 abierto y resultados del monitor serie](imagenes/05_configuracion_camara.png)

El editor muestra `CameraWebServer1.ino` y las pestañas `app_httpd.cpp`, `board_config.h`, `camera_index.h` y `camera_pins.h`. Se ven las bibliotecas de cámara y Wi-Fi, la referencia a la configuración de placa y la inicialización del puerto serie a **115200 baudios**.

En el monitor serie permanecen visibles las siguientes puntuaciones:

| Lectura visible | Botella | Mouse |
|---|---:|---:|
| 1 | 88.672 % | 11.328 % |
| 2 | 84.375 % | 15.625 % |
| 3 | 91.016 % | 8.984 % |

En las tres lecturas la clase dominante es Botella. Sin embargo, **tener abierto el ejemplo de servidor de cámara no demuestra que ese programa produzca las predicciones**: el monitor serie muestra la salida del firmware que esté ejecutándose en la placa. La captura tampoco acredita una prueba de SenseCraft ni muestra el servidor web funcionando en un navegador.

## 6. Tres registros de prueba y análisis de confianza

Se selecciona una lectura de cada una de las imágenes 2, 3 y 4 para resumir los resultados observados. Son **registros de inferencia**, no tres pruebas estadísticas independientes de desempeño.

| Registro | Evidencia | Clase dominante | Confianza dominante | Confianza de la otra clase |
|---|---|---|---:|---:|
| 1 | Imagen 2 | Botella | 98.828 % | 1.172 % |
| 2 | Imagen 3 | Mouse | 99.609 % | 0.391 % |
| 3 | Imagen 4, primera lectura | Botella | 96.484 % | 3.516 % |

La conversión utilizada es:

**Confianza en porcentaje = puntuación del modelo × 100**

Por ejemplo: `0.99609 × 100 = 99.609 %`.

La media descriptiva de las tres puntuaciones dominantes seleccionadas es **98.307 %**; el mínimo es **96.484 %** y el máximo es **99.609 %**. Esta media describe únicamente esos registros seleccionados: **no es la exactitud del modelo** ni demuestra su rendimiento sobre imágenes nuevas.

Los tiempos mostrados suman aproximadamente **174 ms** entre DSP y clasificación (`4 + 170 ms`). No representan necesariamente el tiempo completo de captura, procesamiento y comunicación.

### Confianza frente a exactitud

La confianza es la puntuación que el modelo asigna a una clase para una entrada. La exactitud requiere comparar las predicciones con etiquetas reales conocidas en un conjunto de prueba:

**Exactitud (%) = predicciones correctas / total de imágenes evaluadas × 100**

Sin esas etiquetas y resultados completos no se pueden calcular de forma verificable la exactitud, la precisión, el recall ni una matriz de confusión. El valor `Anomaly: 0 ms` es un tiempo mostrado por el programa; no significa una tasa de anomalías de 0 %.

## 7. Estado de las evidencias 

| Requisito de la pizarra | Evidencia disponible | Estado |
|---|---|---|
| Descripción del proyecto | Objetivo, materiales y explicación de capturas | Documentado |
| Dataset: imágenes y etiquetas | Dos ZIP con 50 JPG cada uno; clases explicadas | Adjuntado; revisar nombres internos al importar |
| Modelo comprimido | `gg.zip`, que contiene `gg.lite` | Adjuntado |
| Tres pruebas estadísticas | Tres registros resumidos y estadística descriptiva de sus puntuaciones | Falta evaluación formal si se exige desempeño estadístico |
| Edge Impulse: pruebas del paso 6 | Capturas de inferencia en Arduino con un ejemplo de Edge Impulse | Falta captura del paso 6 en la plataforma |
| SenseCraft: pruebas | No se adjuntó una captura identificable de SenseCraft | Pendiente |



## 9. Conclusiones

- Los archivos entregados incluyen 100 imágenes repartidas en dos conjuntos de igual tamaño y un archivo de modelo comprimido.
- El monitor serie muestra predicciones para Botella y Mouse con puntuaciones dominantes altas en los registros seleccionados.
- Las capturas permiten documentar las salidas del clasificador y sus tiempos informados, pero no medir la exactitud global del sistema.
- Para completar todos los requisitos de la pizarra, se deben incorporar las evidencias del paso 6 de Edge Impulse, las pruebas de SenseCraft y la evaluación estadística solicitada por el docente.

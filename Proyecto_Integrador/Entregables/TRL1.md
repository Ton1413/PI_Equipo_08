# Estado del arte: métodos de caracterización de residuos metálicos
## 1. Introducción

La caracterización de residuos metálicos permite reunir información útil para decidir cómo separar, almacenar, valorizar o reincorporar un material a otro proceso productivo. Puede incluir propiedades físicas como masa y dimensiones, características visuales y, cuando se necesita conocer la composición elemental o el tipo de aleación, análisis instrumentales especializados. La calidad del material depende tanto de su composición como de sus características físicas y de la presencia de materiales no deseados \[1\].

En la industria se emplean desde inspecciones visuales y mediciones físicas hasta tecnologías basadas en sensores, espectrometría y visión artificial. Estas técnicas no son equivalentes: algunas detectan presencia metálica o describen la forma, mientras que otras permiten estimar una clase de material o analizar su composición.

RECONEXA propone una caracterización **preliminar** de muestras metálicas: inicialmente cobre, aluminio y latón mediante una estación física y una plataforma digital de registro. No pretende reemplazar el análisis químico de laboratorio, sino organizar información inicial que facilite la consulta de los materiales por potenciales empresas receptoras.

## 2. Métodos actuales de caracterización

### 2.1 Inspección visual y visión artificial

La inspección visual permite observar color, forma, dimensiones aparentes, oxidación y estado superficial. Es accesible y rápida, pero depende de la experiencia del observador. El color por sí solo no confirma la identidad del metal: la iluminación, suciedad, recubrimientos, oxidación y aleaciones pueden cambiar su apariencia.

La visión artificial emplea cámaras y algoritmos para extraer características y, si existe un modelo entrenado, clasificar objetos. Koinig et al. compararon veinte arquitecturas de redes neuronales convolucionales para clasificar chatarra con contenido de cobre a partir de imágenes RGB. Los resultados variaron según la arquitectura y el hardware disponible, mostrando potencial para utilizar cámaras de menor costo como apoyo a la clasificación \[2\]. Otro estudio aplicó visión artificial y aprendizaje automático para detectar elementos no permitidos en una línea de clasificación de chatarra basada en cobre, destacando la necesidad de evaluar el sistema en condiciones próximas a las reales \[3\].

Estos estudios no significan que cualquier cámara identifique por sí sola la composición química. Un modelo de visión debe entrenarse y validarse con muestras representativas. La iluminación, perspectiva, suciedad, oxidación y variedad de aleaciones pueden afectar su desempeño.

### 2.2 Mediciones físicas: masa, dimensiones y volumen estimado

Las mediciones físicas describen la muestra sin determinar necesariamente su composición. Una celda de carga permite medir la masa mediante la señal asociada al peso. Un sensor de distancia puede estimar una dimensión, como la altura de una pieza sobre una plataforma. Si se conocen varias dimensiones y la geometría puede aproximarse a una forma sencilla, es posible estimar el volumen; para piezas irregulares, esa
estimación requiere supuestos adicionales.

Estas variables son útiles para inventariar materiales y comparar cantidades disponibles. Sin embargo, masa, altura o volumen no identifican de forma única un metal: piezas de distintos materiales pueden compartir dimensiones y masas semejantes. Por tanto, las mediciones físicas son datos complementarios, no una prueba definitiva de identidad.

### 2.3 Sensores inductivos y técnicas electromagnéticas

Los sensores inductivos detectan objetos metálicos mediante la interacción entre un campo electromagnético y el material conductor. En una aplicación de proximidad, la salida indica presencia dentro de un rango de operación; por sí sola, no distingue con certeza entre cobre, aluminio y latón.

Las técnicas más avanzadas basadas en inducción electromagnética pueden aportar información adicional. Williams, O'Toole y Peyton estudiaron la clasificación estos residuos mediante espectroscopia de inducción magnética combinada con visión artificial. Su investigación encontró que combinar información electromagnética y color podía mejorar la separación de ciertos flujos de residuos, aunque el resultado dependía de las características de las muestras \[4\].

Para RECONEXA es esencial distinguir entre **detectar que hay metal** y **estimar qué tipo de metal podría ser**. Un sensor inductivo deproximidad puede apoyar la detección de presencia metálica; una identificación más específica requiere señales discriminantes, calibración y validación con muestras conocidas.

### 2.4 Técnicas instrumentales para identificar composición

Cuando se necesita conocer la composición elemental, pueden utilizarse técnicas como la fluorescencia de rayos X (XRF) y la espectrometría de emisión óptica por plasma inducido por láser (LIBS). Su propósito es
distinto al de una cámara o un sensor de proximidad.

-   **XRF:** analiza la radiación característica emitida por una muestra  excitada con rayos X y permite estimar la presencia de determinados elementos. Se usa en identificación de aleaciones y clasificación de este material. Los resultados dependen del equipo, calibración, geometría, superficie y condiciones de medición.
-   **LIBS:** utiliza un pulso láser para generar un pequeño plasma y analiza la luz emitida por los elementos presentes. Puede aportar información elemental con rapidez, pero depende de las condiciones
    de adquisición, superficie y calibración.

### 2.5 Sistemas automatizados de clasificación

Las instalaciones industriales pueden combinar cámaras, sensores electromagnéticos y otros sistemas con algoritmos de clasificación y mecanismos de separación. Además de la precisión, son importantes la velocidad de procesamiento, el rendimiento del flujo y la estabilidad frente a cambios en condiciones reales.

Este estudio recientemente menciona el aprendizaje profundo para reconocer y apoyar su clasificación \[2\], \[3\]. También se han investigado modelos que estiman simultáneamente la masa y la clase de objetos a partir de datos visuales \[5\]. Los resultados de un estudio no deben generalizarse automáticamente a cualquier sistema: dependen del conjunto de datos, las clases definidas, el entorno y el procedimiento de evaluación.

## 3. Análisis crítico del estado del arte

No existe una única técnica que resuelva todas las necesidades. La inspección visual y las cámaras aportan información sobre apariencia, pero no determinan directamente la composición química. Las mediciones de masa y dimensiones cuantifican propiedades físicas, aunque no distinguen por sí solas entre metales con características semejantes. Los sensores inductivos convencionales pueden detectar presencia metálica, pero no equivalen a un analizador de aleaciones.

Los estudios sobre visión artificial y espectroscopia de inducción muestran que combinar fuentes de información puede mejorar la clasificación en escenarios específicos \[2\]--\[5\]. Sin embargo, el desempeño depende de las muestras, las clases, el equipo y el entorno de prueba. Antes de aplicar un método en otro contexto, debe evaluarse con muestras representativas.
## 4. Sensores y sistemas de medición para residuos metálicos

Los sensores y sistemas de medición permiten conocer algunas características de los residuos metálicos, como su peso, tamaño y tipo de material. Cada tecnología cumple una función diferente: por ejemplo, las celdas de carga miden el peso, los sensores inductivos detectan la presencia de metales y las cámaras permiten observar la forma y apariencia de las piezas [6], [7]. 

La elección de estos dispositivos depende del tipo de residuo que se desea identificar, la precisión necesaria y el costo del sistema. Por ello, combinar diferentes sensores puede ayudar a obtener más información y mejorar la clasificación de los materiales. Sin embargo, es importante considerar que detectar un metal no significa necesariamente conocer si es cobre, aluminio o latón [8].
En RECONEXA se utilizan diferentes tecnologías para reunir estos datos y apoyar la clasificación de los materiales. Cada dispositivo cumple una función específica, por lo que es importante conocer sus características y limitaciones [9]–[10].

**Tabla 1. Sensores utilizados en el proyecto RECONEXA**

| Sensor o dispositivo | Función principal | Aplicación en RECONEXA |
|---|---|---|
| XIAO ESP32-S3 Sense con cámara OV3660 [6] | Capturar imágenes y procesar información; cuenta con Wi-Fi, BLE y micrófono. | Registrar la apariencia de los residuos y permitir la comunicación del sistema. |
| Sensor inductivo de dos canales LDC1612 [7] | Medir cambios de inductancia relacionados con objetos metálicos. | Obtener señales que ayuden a diferenciar características de los metales. |
| Celda de carga (sensor de peso) [8] | Medir el peso de los objetos. | Registrar la masa de los residuos metálicos. |
| Sensor de efecto Hall [9] | Detectar campos magnéticos. | Identificar respuestas magnéticas que complementen la caracterización. |
| Sensor de distancia ToF VL53L0X [10] | Medir la distancia mediante el tiempo de vuelo de la luz. | Estimar la distancia y las dimensiones aproximadas de los residuos. |

La combinación de estos dispositivos permite obtener información complementaria de cada residuo. La cámara registra su apariencia, la celda de carga mide su peso, el VL53L0X estima distancias y los sensores inductivo y Hall aportan información sobre sus propiedades electromagnéticas y magnéticas. En conjunto, estas mediciones pueden apoyar la clasificación de los metales, aunque es necesario evaluar los resultados con muestras conocidas para comprobar su capacidad de diferenciación.
## 5. RECONEXA

RECONEXA busca registrar información de muestras metálicas y facilitar su consulta posterior. La combinación propuesta puede generar una ficha inicial de cada muestra:

-   **Masa:** cuantifica la cantidad de material.
-   **Distancia o altura estimada:** aporta una característica geométrica.
-   **Detección inductiva:** apoya la detección de presencia metálica, pero no confirma por sí sola si es cobre, aluminio o latón.
-   **Imagen:** documenta el aspecto visible y podría apoyar una clasificación visual si se desarrolla y valida un modelo apropiado.
-   **Registro digital:** organiza los resultados para consultarlos y compararlos.

Cada variable describe una dimensión distinta de la muestra. No obstante, combinar mediciones no garantiza automáticamente una clasificación correcta. Será necesario definir el procedimiento, calibrar sensores, establecer criterios y evaluar el prototipo con muestras conocidas de cobre, aluminio y latón. La exactitud de la identificación debe demostrarse experimentalmente.

Por tanto, RECONEXA se plantea como una herramienta de **caracterización preliminar y organización de información**, no como sustituto de XRF, LIBS u otros análisis de laboratorio. Si un proceso industria requiere conocer la composición exacta, los resultados deberían complementarse con una técnica analítica adecuada.


## Referencias

\[1\] CAESAR Project, "Scrap characterization methods," 5 oct. 2024. https://caesarproject.eu/report-on-scrap-characterization-methods/.
\[26-oct-2026\].

\[2\] G. Koinig, N. Kuhn, T. Fink, B. Lorber, Y. Radmann, W. Martinelli y A. Tischberger-Aldrian, "Deep learning approaches for classification of copper-containing metal scrap in recycling processes," *Waste
Management*, vol. 190, pp. 520--530, dic. 2024, doi: 10.1016/j.wasman.2024.10.022.

\[3\] T. Jurtsch, J. Moryson y G. Wiczyński, "Machine vision-based detection of forbidden elements in the high-speed automatic scrap sorting line," *Waste Management*, vol. 189, pp. 243--253, dic. 2024,
doi: 10.1016/j.wasman.2024.08.015.

\[4\] K. Williams, M. O'Toole y A. Peyton, "Scrap metal classification using magnetic induction spectroscopy and machine vision,"Transactions on Instrumentation and Measurement*, vol. 72, art. no. 2520211, 2023, doi: 10.1109/TIM.2023.3284930.
\[5\] D. J. Díaz-Romero *et al*., "Simultaneous mass estimation and class classification of scrap metals using deep learning," *Resources, Conservation and Recycling*, vol. 181, art. no. 106272, 2022, doi: 10.1016/j.resconrec.2022.106272.
[6] Seeed Studio, “Getting Started with Seeed Studio XIAO ESP32-S3,” *Seeed Studio Wiki*. [En línea]. Disponible en: https://wiki.seeedstudio.com/xiao_esp32s3_getting_started/
[7] National Instruments, “Measuring Load with Bridge-Based Sensors.” [En línea]. Disponible en: https://www.ni.com/en/shop/data-acquisition/sensor-fundamentals/measuring-load-with-bridge-based-sensors.html
[8] Texas Instruments, “What Is a Hall-Effect Sensor?” [En línea]. Disponible en: https://www.ti.com/document-viewer/lit/html/SSZT164
[9] STMicroelectronics, “VL53L0X: Time-of-Flight Ranging Sensor.” [En línea]. Disponible en: https://www.st.com/en/imaging-and-photonics-solutions/vl53l0x.html
\[10\] D. J. Díaz-Romero *et al*., "Simultaneous mass estimation and class classification of scrap metals using deep learning," *Resources, Conservation and Recycling*, vol. 181, art. no. 106272, 2022, doi: 10.1016/j.resconrec.2022.106272.


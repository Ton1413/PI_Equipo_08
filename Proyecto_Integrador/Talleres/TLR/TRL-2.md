# TRL 2: Formulación del concepto tecnológico
## 1. Descripción de la propuesta tecnológica
RECONEXA es una propuesta tecnológica para caracterizar preliminarmente residuos metálicos y generar información que facilite su aprovechamiento y redistribución entre empresas. El concepto combina una estación de medición de sobremesa y un sistema de registro y consulta de datos, orientado a la simbiosis industrial y a la economía circular.

La caracterización propuesta considera muestras de cobre, aluminio y latón. Las funciones contemplan la captura de imágenes, la medición del peso, la estimación de dimensiones o volumen y la medición de la respuesta inductiva. Los resultados se destinan a la identificación estimada del material y a su registro digital. Estas son funciones de diseño; su desempeño no se considera validado en TRL 2.
## 2. Objetivo del TRL 2
Formular el concepto tecnológico de RECONEXA mediante el análisis de antecedentes científicos y de propiedad intelectual, la definición de funciones, la evaluación de alternativas técnicas y la representación tridimensional de la propuesta.
## 3. Perfil y aplicación de la propuesta
La propuesta aborda la necesidad de disponer de información sobre residuos metálicos para facilitar su posible reutilización como insumos de otras empresas. RECONEXA contempla un procedimiento de recepción y posicionamiento de la muestra, adquisición de señales, procesamiento de información y presentación de resultados.

Su aplicación prevista comprende la caracterización preliminar de pequeñas muestras metálicas y el registro de información útil para vincular la oferta de materiales con potenciales usuarios industriales. La precisión de clasificación y la viabilidad de redistribución requieren comprobaciones posteriores.
## 6. Matriz morfológica y selección de alternativas
<img width="2563" height="8284" alt="649838340-8cabfea4-3fb6-46f6-9e46-c166249f5e75" src="https://github.com/user-attachments/assets/69661b5e-d323-4b8b-899d-c973fd82bdda" />

Figura 3. Matriz morfológica. Fuente: elaboración del equipo RECONEXA .

La matriz reúne alternativas para la recepción de la muestra, inicio del ciclo, medición, procesamiento, clasificación, comunicaciones, almacenamiento, visualización, alimentación y protección física. Constituye evidencia de la evaluación conceptual de soluciones técnicas. Las alternativas reflejan la etapa de diseño y no equivalen a una lista de componentes experimentalmente validados.

## 7. Boceto 
Esta propuesta presenta un diseño en el que los componentes electrónicos y sensores se distribuyen sobre una estructura que permite realizar la caracterización preliminar de muestras metálicas.

El sistema cuenta con una plataforma de pesaje conectada a una celda de carga, encargada de medir la masa de la muestra, mientras que el módulo HX711 permite procesar la señal obtenida. Asimismo, incorpora un sensor ToF, ubicado en la parte superior mediante un brazo de soporte, para estimar la distancia hacia la muestra, y un sensor inductivo para detectar la presencia de materiales metálicos.

Los datos obtenidos son procesados mediante un microcontrolador ESP32, ubicado dentro de un gabinete que también integra una pantalla LCD para visualizar las mediciones y un botón de inicio para activar el sistema.

Esta configuración busca optimizar el espacio, facilitar la integración de los componentes y permitir una operación sencilla, constituyendo una alternativa conceptual para el desarrollo del prototipo RECONEXA.
	
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5d403ee3-168f-4862-a4b4-b017334c99a1" />

Figura 4. Boceto de la alternativa compacta del sistema RECONEXA, con plataforma de pesaje, sensores de caracterización y gabinete de control electrónico. Fuente: elaboración propia.

## 8. Modelado tridimensional
Vista isométrica	Vista frontal	Vista superior
<img width="755" height="633" alt="image" src="https://github.com/user-attachments/assets/6db230e3-3f61-4c9e-8146-86473e6d9726" />

		


Figura 5. Modelado 3D de RECONEXA. Fuente: elaboración del equipo RECONEXA .

El modelado presenta una plataforma para posicionar muestras, un soporte elevado para el módulo de medición y una sección lateral destinada a elementos electrónicos y de interfaz. Esta representación establece la geometría conceptual y la distribución espacial del dispositivo.
## 9. Estado del arte: antecedentes científicos
Antecedente	Aporte relacionado	Comparación con RECONEXA
Multilayer Hybrid Deep-Learning Method for Waste Classification and Recycling (2018) [1]	Clasificación de residuos a partir de información de sensores y técnicas de aprendizaje profundo.	Coincide con el enfoque de adquisición de diferentes características de los residuos; RECONEXA se centra en la caracterización preliminar de metales y el registro para redistribución.
Kutila et al., Scrap Metal Sorting with Colour Vision and Inductive Sensor Array (2005) [2]	Estudio de clasificación de chatarra mediante visión de color y sensores inductivos.	Coincide con el análisis de señales visuales e inductivas de metales; RECONEXA agrega funciones previstas de pesaje y registro digital.


## 10. Antecedente de patente y comparación tecnológica
Se identifica el documento de patente WO2024207048A1, Waste Sorting Method and Apparatus (2024) [3], relacionado con la clasificación de residuos mediante un aparato de detección. Su revisión permite contrastar la finalidad y la arquitectura de clasificación con la propuesta de RECONEXA.

Dibujos originales de la patente: Consultar figuras de WO2024207048A1 en Google Patents. Los dibujos originales no se reproducen en este repositorio; el enlace permite consultar la fuente primaria.
Antecedente	Semejanza funcional con RECONEXA	Diferencia principal	Evidencia visual
WO2024207048A1 [3]	Detección y clasificación de residuos.	RECONEXA plantea una estación de caracterización preliminar con registro orientado a redistribución industrial.	Figuras originales de patente
Artículo Multilayer Hybrid [1]	Uso combinado de información para clasificar residuos.	RECONEXA enfoca su propuesta en metales y datos para intercambio industrial.	Figuras del artículo
Kutila et al. [2]	Visión e inducción aplicadas a residuos metálicos.	RECONEXA contempla además pesaje, interfaz y registro digital.	Publicación original
RECONEXA (2026)	Integra conceptualmente las funciones anteriores.	Diseño propio de sobremesa y aplicación de redistribución.	


Tabla 1. Comparación de antecedentes con el concepto tecnológico RECONEXA. Los enlaces conducen a las figuras publicadas por sus titulares; la imagen de RECONEXA es del equipo.
## 11. Análisis preliminar de patentabilidad y protección

RECONEXA identificó la patente **WO2024207048A1**, relacionada con la detección y clasificación de residuos [3]. También se encontraron tecnologías similares que utilizan cámaras y sensores inductivos.

Nuestra propuesta combina la caracterización de metales, una estructura de sobremesa y el registro digital para facilitar su redistribución. Sin embargo, esta combinación no garantiza que sea patentable, por lo que aún debe evaluarse su novedad.

Actualmente, RECONEXA no cuenta con una patente propia acreditada. Esto representa un riesgo de copia de sus elementos innovadores. Además, se identificó la necesidad de definir la autoría y revisar las patentes existentes para evitar posibles conflictos legales.

## 12. Riesgos de propiedad intelectual

| Riesgo identificado | Consecuencia |
|---|---|
| Falta de patente propia | Posible copia de la innovación. |
| Existencia de patentes similares | Posibles conflictos legales. |
| Falta de registro de marca | Uso del nombre RECONEXA por terceros. |
| Falta de acuerdos sobre la propiedad | Conflictos entre los titulares del proyecto. |

Estos riesgos forman parte de las barreras identificadas para el futuro escalamiento de RECONEXA.

## 13. Normas y regulaciones identificadas

RECONEXA identifica las siguientes normas y regulaciones relacionadas con su desarrollo y futura comercialización.

| Norma o regulación | Aplicación en RECONEXA |
|---|---|
| ISO 16290:2013 | Evaluación de la madurez tecnológica. |
| ISO 31000:2018 | Gestión de riesgos del proyecto. |
| ISO 9001:2015 | Control de calidad del sistema. |
| ISO 14001:2015 | Gestión de impactos ambientales. |
| ISO 45001:2018 | Seguridad durante la operación. |
| D. Leg. N.° 1278 | Gestión de residuos sólidos en Perú. |
| Decisión Andina 486 | Protección de la propiedad industrial. |

Estas referencias forman parte de los requisitos identificados desde TRL 2. Su aplicación específica corresponde a las siguientes etapas de desarrollo.

## 14. Resultado del TRL 2

RECONEXA cuenta con una propuesta tecnológica definida mediante la matriz morfológica, los bocetos, el modelado 3D y el análisis de antecedentes científicos.

Se identificó una patente relacionada con la clasificación de residuos, cuatro riesgos de propiedad intelectual y siete referencias normativas para el futuro escalamiento.

Estas evidencias corresponden a la formulación conceptual de RECONEXA y sirven como base para continuar su desarrollo tecnológico.

## 15. Referencias bibliográficas (IEEE)

[1] “Multilayer Hybrid Deep-Learning Method for Waste Classification and Recycling,” 2018. [En línea]. Disponible: https://pmc.ncbi.nlm.nih.gov/articles/PMC6236983/

[2] M. Kutila et al., “Scrap Metal Sorting with Colour Vision and Inductive Sensor Array,” 2005. [En línea]. Disponible: https://cris.vtt.fi/en/publications/scrap-metal-sorting-with-colour-vision-and-inductive-sensor-array/

[3] “Waste Sorting Method and Apparatus,” patente WO2024207048A1, 2024. [En línea]. Disponible: https://patents.google.com/patent/WO2024207048A1/en

[4] ISO, *ISO 16290:2013, Space systems—Definition of the Technology Readiness Levels (TRLs) and their criteria of assessment*, 2013.

[5] ISO, *ISO 31000:2018, Risk management—Guidelines*, 2018.

[6] ISO, *ISO 9001:2015, Quality management systems—Requirements*, 2015.

[7] ISO, *ISO 14001:2015, Environmental management systems—Requirements with guidance for use*, 2015.

[8] ISO, *ISO 45001:2018, Occupational health and safety management systems—Requirements with guidance for use*, 2018.

[9] Perú, *Decreto Legislativo N.° 1278, Ley de Gestión Integral de Residuos Sólidos*, 2016.

[10] Comunidad Andina, *Decisión 486, Régimen Común sobre Propiedad Industrial*, 2000.

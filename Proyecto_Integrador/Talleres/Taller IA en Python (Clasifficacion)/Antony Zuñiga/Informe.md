# Análisis de regresión lineal de la concentración de ozono troposférico en Nueva York durante 2022 y 2023

## Introducción

El ozono a nivel del suelo o troposférico ($O_3$) no se emite directamente a la atmósfera, sino que se forma mediante reacciones químicas entre óxidos de nitrógeno ($NO_x$) y compuestos orgánicos volátiles ($VOC$) en presencia de luz solar [1]. A diferencia de la capa de ozono estratosférico que protege la Tierra de la radiación ultravioleta, el ozono troposférico es un contaminante nocivo que reduce la función pulmonar, desencadena ataques de asma e incrementa los ingresos hospitalarios por afecciones respiratorias [1].

La Agencia de Protección Ambiental de los Estados Unidos (EPA) monitorea y recopila de manera continua las concentraciones de este contaminante a través del sistema Air Quality System (AQS) [2]. Mediante la plataforma de consulta de datos diarios es posible acceder a métricas estandarizadas como el valor diario del Índice de Calidad del Aire (AQI) y la concentración máxima diaria de ozono promediada en períodos de ocho horas [3].

En este trabajo se analizaron los registros diarios de la concentración máxima de ozono en ocho horas (expresada en partes por millón, ppm) y su correspondiente AQI para el estado de Nueva York durante el periodo continuo de 2022 y 2023. El objetivo general fue evaluar la capacidad de un modelo de regresión lineal para estimar el valor diario del AQI a partir de las concentraciones del contaminante y la incorporación de variables de ingeniería de características (*feature engineering*) orientadas a capturar patrones temporales y estacionales.

## Metodología

### Fuente y preparación de los datos

Se utilizaron los archivos `1ozono2022.csv` y `1ozono2023.csv`, extraídos de la herramienta *Download Daily Data* de la EPA. Ambos conjuntos de datos se consolidaron mediante una operación de concatenación longitudinal, construyendo una serie temporal continua para los años 2022 y 2023. 

El procesamiento de datos se ejecutó en Python. La columna `Date` se transformó al formato de fecha estandarizado (`%m/%d/%Y`) y se ordenó cronológicamente la serie. No se detectaron valores nulos en la variable predictora principal (`Daily Max 8-hour Ozone Concentration`) ni en la variable de respuesta (`Daily AQI Value`).

### Ingeniería de características

A partir de la columna de fecha se extrajeron componentes temporales para considerar la estacionalidad del ozono, la cual tiende a presentar picos durante los meses de verano debido al incremento de la radiación solar y la temperatura. Se construyeron las siguientes variables explicativas:

- `Daily Max 8-hour Ozone Concentration`: concentración máxima diaria de ozono en 8 horas (ppm).
- `Mes`: mes de la observación ($1 \le \text{Mes} \le 12$).
- `Dia_del_Anio`: día consecutivo del año ($1 \le \text{DiaDelAnio} \le 366$).
- `Trimestre`: trimestre del año ($1 \le \text{Trimestre} \le 4$).
- `Anio`: año correspondiente a la observación ($2022$ o $2023$).

La variable objetivo ($Y$) del modelo se definió como `Daily AQI Value`.

### Modelo de regresión y partición de datos

Para modelar la relación entre las características explicativas y el índice del aire se utilizó la clase `LinearRegression` de la librería *scikit-learn*, la cual encuentra los coeficientes que minimizan la suma de errores cuadráticos ordinarios (OLS) [4].

El modelo múltiple de regresión lineal se expresó de la siguiente forma:

$$AQI_i = \beta_0 + \beta_1 X_{1,i} + \beta_2 \text{Mes}_i + \beta_3 \text{DiaDelAnio}_i + \beta_4 \text{Trimestre}_i + \varepsilon_i$$

donde $X_1$ representa la concentración máxima diaria de ozono en 8 horas y $\varepsilon_i$ el término de error aleatorio.

La evaluación se realizó mediante una división aleatoria del conjunto consolidado en un **80% para entrenamiento** y un **20% para prueba**, fijando una semilla de reproducibilidad (`random_state=42`).

### Métricas de evaluación

El desempeño del modelo se evaluó en el conjunto de prueba utilizando el coeficiente de determinación ($R^2$), el error cuadrático medio (MSE), la raíz del error cuadrático medio (RMSE) y el error absoluto medio (MAE) [5]. 

$$R^2 = 1 - \frac{\sum_{i=1}^{n} (y_i - \hat{y}_i)^2}{\sum_{i=1}^{n} (y_i - \bar{y})^2}$$

$$RMSE = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}$$

$$MAE = \frac{1}{n} \sum_{i=1}^{n} \vert{}y_i - \hat{y}_i\vert{}$$

## Resultados

### Evaluación en el conjunto de prueba

El modelo de Regresión Lineal entrenado con las observaciones del periodo 2022-2023 mostró una capacidad predictiva elevada para estimar el valor del AQI a partir de las concentraciones de ozono y los componentes temporales.

| Métrica de Evaluación | Conjunto de Prueba |
|---|---:|
| Coeficiente de Determinación ($R^2$) | **0.9852** |
| Error Cuadrático Medio (MSE) | **2.0164** |
| Raíz del Error Cuadrático Medio (RMSE) | **1.4200** |
| Error Absoluto Medio (MAE) | **0.9800** |

El coeficiente de determinación ($R^2 = 0.9852$) indicó que el modelo explica aproximadamente el **98.52 %** de la variabilidad del índice de calidad del aire. Las métricas de error mostraron desviaciones medias bajas: un MAE de **0.98 unidades de AQI** y un RMSE de **1.42 unidades de AQI**, lo que refleja un ajuste preciso entre las predicciones del modelo ($\hat{y}$) y los valores observados ($y$).

## Discusión

Los resultados obtenidos confirman una relación highly lineal y directa entre la concentración máxima de ozono en 8 horas y el valor diario del AQI. Esto es coherente con la metodología oficial de la EPA, donde el AQI se calcula a partir de ecuaciones lineales por tramos basadas en concentraciones umbral (*breakpoints*) [2]. 

La inclusión de datos consolidados de dos años consecutivos (2022 y 2023) redujo el sesgo que podría introducir un único año expuesto a condiciones meteorológicas atípicas. Asimismo, la incorporación del mes y día del año ayudó a capturar la variabilidad de fondo asociada a los patrones estacionales donde las concentraciones de $O_3$ se incrementan durante las épocas con mayor radiación solar.

Entre las limitaciones del modelo, cabe señalar que las ecuaciones de conversión oficial del AQI utilizan funciones lineales definidas por partes con cambios de pendiente en cada umbral de riesgo para la salud. Por ello, en las zonas de transición entre categorías de calidad del aire (por ejemplo, al pasar de "Buena" a "Moderada"), un modelo de regresión lineal global único puede generar pequeñas desviaciones en los extremos de la distribución.

## Referencias

[1] U.S. Environmental Protection Agency, “Ground-level Ozone Basics.” [En línea]. Disponible en: https://www.epa.gov/ground-level-ozone-pollution/ground-level-ozone-basics. [Accedido: 18-sep-2026].

[2] U.S. Environmental Protection Agency, “Technical Assistance Document for the Reporting of Daily Air Quality – the Air Quality Index (AQI),” *EPA-454/B-18-007*, Sep. 2018.

[3] U.S. Environmental Protection Agency, “Download Daily Data.” [En línea]. Disponible en: https://www.epa.gov/outdoor-air-quality-data/download-daily-data. [Accedido: 18-sep-2026].

[4] Scikit-learn Developers, “LinearRegression,” *scikit-learn documentation*. [En línea]. Disponible en: https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html. [Accedido: 18-sep-2026].

[5] Scikit-learn Developers, “Metrics and scoring: Quantifying the quality of predictions,” *scikit-learn documentation*. [En línea]. Disponible en: https://scikit-learn.org/stable/modules/model_evaluation.html#regression-metrics. [Accedido: 18-sep-2026].

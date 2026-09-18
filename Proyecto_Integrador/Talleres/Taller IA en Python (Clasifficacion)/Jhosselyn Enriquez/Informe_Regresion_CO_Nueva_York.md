# Análisis de regresión lineal de la concentración de monóxido de carbono en Nueva York durante 2022

## Introducción

El monóxido de carbono (CO) es un gas incoloro e inodoro generado principalmente por procesos de combustión. En el ambiente exterior, los vehículos y otras maquinarias que utilizan combustibles fósiles constituyen algunas de sus fuentes más importantes [1]. En concentraciones elevadas, el CO disminuye la capacidad de la sangre para transportar oxígeno hacia órganos como el corazón y el cerebro, por lo que su vigilancia es relevante para evaluar la calidad del aire y prevenir riesgos para la salud [1].

La Agencia de Protección Ambiental de los Estados Unidos (EPA) almacena información de calidad del aire procedente de redes de monitoreo federales, estatales, locales y tribales mediante el sistema Air Quality System (AQS) [2]. La plataforma de datos diarios permite consultar estadísticas de los contaminantes criterio por estación, ciudad, condado o estado [3].

En este trabajo se analizaron los registros diarios de la concentración máxima de CO en ocho horas, expresada en partes por millón (ppm), correspondientes al estado de Nueva York durante 2022. El objetivo fue describir el comportamiento temporal y espacial del contaminante y construir modelos de regresión lineal capaces de estimar su concentración diaria. Se comparó un modelo simple basado únicamente en el tiempo con un modelo múltiple que incorporó tendencia, estacionalidad, tipo de día y persistencia temporal.

## Metodología

### Fuente y preparación de los datos

Se utilizó el archivo `Data1_CO_New York.csv`, descargado de la herramienta *Download Daily Data* de la EPA. El conjunto original presentó 3596 observaciones y 21 variables. Entre las columnas disponibles se encontraron la fecha, la estación de monitoreo, el condado, la concentración máxima diaria de CO en ocho horas, el valor diario del índice de calidad del aire (AQI), el número de observaciones y el porcentaje de datos completos.

El procesamiento se realizó en Python mediante Google Colab. Primero, la columna `Date` se convirtió al tipo fecha y se verificó la existencia de valores faltantes en las variables principales. No se encontraron registros nulos en la fecha, la concentración de CO, el AQI, el nombre de la estación ni el condado. Posteriormente, se construyó una serie temporal única calculando el promedio de las estaciones para cada día. Como resultado, se obtuvieron 365 observaciones diarias entre el 1 de enero y el 31 de diciembre de 2022.

El AQI se mantuvo únicamente como variable descriptiva. No se utilizó para estimar la concentración porque se obtiene a partir de la concentración del contaminante; incluirlo como predictor produciría redundancia y un ajuste artificialmente alto.

### Análisis exploratorio

El análisis exploratorio incluyó:

1. Histograma y curva de densidad de la concentración diaria de CO.
2. Serie temporal diaria con promedio móvil de siete días.
3. Perfil mensual con promedio y desviación estándar.
4. Comparación de la concentración promedio entre estaciones.
5. Matriz de correlación de las variables del modelo.
6. Diagrama de dispersión entre el CO actual y la concentración del día anterior.

Estas visualizaciones permitieron revisar la distribución de los datos, reconocer patrones estacionales, comparar las estaciones y evaluar la persistencia temporal del contaminante.

### Construcción de variables

Se generaron las siguientes variables explicativas:

- `t`: días transcurridos desde el comienzo del periodo.
- `sin_anual` y `cos_anual`: componentes trigonométricos que representan un ciclo estacional anual.
- `fin_de_semana`: variable binaria igual a 1 para sábado o domingo y 0 para los demás días.
- `CO_dia_anterior`: concentración promedio de CO registrada el día previo.

El primer día fue excluido del modelado porque no contaba con una observación anterior. Por esta razón, los modelos emplearon 364 registros diarios.

### Modelos de regresión

Se construyeron dos modelos con la clase `LinearRegression` de *scikit-learn*, que ajusta una regresión mediante mínimos cuadrados ordinarios [4].

El modelo simple se definió como:

$$
CO_t = \beta_0 + \beta_1t + \varepsilon_t
$$

El modelo múltiple se definió como:

$$
CO_t = \beta_0 + \beta_1t + \beta_2\sin(d_t) + \beta_3\cos(d_t)
+ \beta_4F_t + \beta_5CO_{t-1} + \varepsilon_t
$$

donde $F_t$ identifica los fines de semana y $CO_{t-1}$ representa la concentración del día anterior.

Debido a que las observaciones forman una serie temporal, la división entre entrenamiento y prueba se realizó cronológicamente. Se usaron los datos de enero a septiembre para entrenar los modelos y los datos de octubre a diciembre para evaluarlos. De esta manera se evitó que información futura ingresara al conjunto de entrenamiento.

### Métricas de evaluación

El desempeño se evaluó mediante el coeficiente de determinación ($R^2$), la raíz del error cuadrático medio (RMSE) y el error absoluto medio (MAE), métricas empleadas para cuantificar la calidad de predicciones de regresión [5]. El $R^2$ mide la proporción de variabilidad explicada; en el conjunto de prueba puede ser negativo si el modelo predice peor que el valor medio. El RMSE y el MAE se expresaron en ppm, por lo que valores menores indicaron mayor precisión.

## Resultados

### Descripción de la concentración de CO

La concentración promedio de la serie diaria fue de **0.2974 ppm**, mientras que el valor máximo diario promedio alcanzó **0.7500 ppm**. La distribución presentó una mayor concentración de observaciones en los valores bajos y una extensión hacia concentraciones superiores, lo que evidenció asimetría positiva.

El análisis mensual reveló concentraciones menores durante el verano y mayores en los últimos meses del año. Agosto presentó el promedio mensual más bajo, con **0.2407 ppm**, mientras que octubre registró el más alto, con **0.3629 ppm**. Esta variación indica que una tendencia lineal por sí sola no representa adecuadamente el comportamiento anual.

También se observaron diferencias espaciales importantes. `PINNACLE STATE PARK` presentó el promedio más bajo, con aproximadamente **0.125 ppm**, mientras que `Queens College Near Road` alcanzó el promedio más alto, con aproximadamente **0.434 ppm**. El valor de la estación cercana a la vía fue alrededor de 3.5 veces el observado en Pinnacle State Park.

### Ajuste con todos los datos

El modelo simple, ajustado sobre la serie completa, obtuvo un $R^2$ de **0.0079** y un RMSE de **0.0971 ppm**. Por lo tanto, el paso del tiempo explicó menos del 1 % de la variabilidad diaria del CO.

El modelo múltiple mejoró el ajuste sobre la serie completa, alcanzando un $R^2$ de **0.3935** y un RMSE de **0.0760 ppm**. El coeficiente asociado a la concentración del día anterior fue **0.5297**, el mayor entre las variables incluidas. Este resultado mostró que la persistencia diaria fue un componente importante para explicar la concentración actual.

### Evaluación en el periodo de prueba

| Modelo | $R^2$ de prueba | RMSE (ppm) | MAE (ppm) |
|---|---:|---:|---:|
| Regresión simple | -1.0872 | 0.1967 | 0.1433 |
| Regresión múltiple | 0.0950 | 0.1295 | 0.0926 |

El modelo simple obtuvo un $R^2$ negativo, lo que indica que su capacidad predictiva en octubre-diciembre fue inferior a utilizar directamente el promedio del conjunto de prueba. En cambio, el modelo múltiple presentó un $R^2$ positivo de **0.0950** y redujo el RMSE de 0.1967 a **0.1295 ppm**, equivalente a una disminución aproximada del **34.2 %**. Asimismo, el MAE disminuyó de 0.1433 a **0.0926 ppm**, una reducción cercana al **35.4 %**.

En el modelo múltiple entrenado con los datos de enero a septiembre, el coeficiente de `CO_dia_anterior` fue **0.4162**. Manteniendo constantes las demás variables, un aumento de 0.1 ppm en la concentración del día previo se asoció con un incremento estimado de aproximadamente 0.0416 ppm en el día actual. Sin embargo, este coeficiente representa una asociación predictiva y no demuestra una relación causal.

El análisis de los residuos mostró diferencias entre las predicciones y los valores reales, especialmente en algunos episodios de concentración elevada. Aunque el modelo múltiple siguió mejor las variaciones temporales que el modelo simple, todavía dejó sin explicar una parte considerable de la variabilidad del periodo de prueba.

## Discusión

Los resultados muestran que la concentración diaria de CO no siguió una tendencia lineal constante durante 2022. El desempeño deficiente del modelo simple se explica porque un solo coeficiente temporal intenta representar simultáneamente fluctuaciones diarias y cambios estacionales. La incorporación de componentes cíclicos, el indicador de fin de semana y la concentración del día anterior mejoró tanto el ajuste como la predicción.

La persistencia diaria fue la variable de mayor contribución dentro del modelo múltiple. Esto significa que el estado reciente de la calidad del aire aportó información útil sobre la concentración del día siguiente. No obstante, el $R^2$ de prueba de 0.0950 evidencia que la capacidad predictiva continuó siendo limitada. La regresión múltiple fue superior a la simple, pero no puede considerarse un modelo de pronóstico completo.

Las diferencias entre estaciones también deben interpretarse con cautela. El promedio estatal reúne sitios urbanos, suburbanos y rurales con características distintas. Una estación próxima al tráfico puede presentar concentraciones superiores a una estación de fondo debido a la proximidad de fuentes móviles; la EPA reconoce a los vehículos y maquinarias que queman combustibles fósiles como fuentes importantes de CO exterior [1]. Sin embargo, el presente análisis no incluyó datos directos de tráfico, por lo que esta explicación debe considerarse una interpretación compatible con los datos y no una comprobación causal.

Entre las principales limitaciones se encuentran el uso de un solo año, la agregación de estaciones diferentes mediante un promedio diario y la ausencia de variables meteorológicas. La temperatura, velocidad y dirección del viento, estabilidad atmosférica, precipitación y volumen de tráfico podrían explicar parte de los errores restantes. Trabajos posteriores podrían utilizar varios años de información, construir modelos específicos por estación e incorporar estos predictores externos.

## Referencias

[1] U.S. Environmental Protection Agency, “Basic Information about Carbon Monoxide (CO) Outdoor Air Pollution.” [En línea]. Disponible en: https://www.epa.gov/co-pollution/basic-information-about-carbon-monoxide-co-outdoor-air-pollution. [Accedido: 18-sep-2026].

[2] U.S. Environmental Protection Agency, “Air Quality System (AQS).” [En línea]. Disponible en: https://www.epa.gov/aqs. [Accedido: 18-sep-2026].

[3] U.S. Environmental Protection Agency, “Download Daily Data.” [En línea]. Disponible en: https://www.epa.gov/outdoor-air-quality-data/download-daily-data. [Accedido: 18-sep-2026].

[4] Scikit-learn Developers, “LinearRegression,” *scikit-learn documentation*. [En línea]. Disponible en: https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html. [Accedido: 18-sep-2026].

[5] Scikit-learn Developers, “Metrics and scoring: Quantifying the quality of predictions,” *scikit-learn documentation*. [En línea]. Disponible en: https://scikit-learn.org/stable/modules/model_evaluation.html#regression-metrics. [Accedido: 18-sep-2026].

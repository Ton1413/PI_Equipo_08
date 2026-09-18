# Regresión Lineal con datos de CO

## Descripción

Se aplicó un modelo de **Regresión Lineal** utilizando una nueva base de datos relacionada con la concentración diaria de **monóxido de carbono (CO)**.

La información contiene datos de mediciones realizadas durante el año 2023 (1 de Enero) hasta (30 de Junio), incluyendo la concentración máxima diaria de CO, características del sitio de monitoreo y algunos datos de las mediciones.

## Objetivo

El objetivo es aplicar técnicas de análisis de datos y regresión lineal para estudiar la relación entre diferentes variables y la **concentración máxima diaria de CO**.

La variable que se busca predecir es:

* `Daily Max CO Concentration`

Para el modelo se utilizaron principalmente variables como:

* `Daily Obs Count`
* `Percent Complete`
* `Site Latitude`
* `Site Longitude`
* `Elevation (m)`
* `Probe Height (m)`

## Proceso realizado

Durante el desarrollo del proyecto se realizaron los siguientes pasos:

1. Carga y exploración de la nueva base de datos.
2. Revisión de las columnas y estadísticas descriptivas.
3. Identificación de valores faltantes.
4. Preparación y limpieza de los datos.
5. Selección de las variables para la regresión.
6. División de los datos en entrenamiento y prueba.
7. Entrenamiento del modelo de **Regresión Lineal**.
8. Predicción de la concentración de CO.
9. Evaluación del modelo mediante métricas como:

   * MSE
   * RMSE
   * MAE
   * R²
10. Visualización de los valores reales y predichos.
11. Análisis de los residuos.
12. Comparación con un modelo de **Árbol de Decisión**.

## Visualizaciones principales

Entre los gráficos realizados se encuentran:

* **Matriz de correlación:** permite observar la relación entre las variables numéricas.
* **Valores reales vs. valores predichos:** permite comparar el resultado del modelo con los valores reales de concentración de CO.
* **Importancia de variables:** permite observar qué variables tienen mayor participación en el modelo de árbol de decisión.

## Herramientas utilizadas

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Statsmodels
* Google Colab

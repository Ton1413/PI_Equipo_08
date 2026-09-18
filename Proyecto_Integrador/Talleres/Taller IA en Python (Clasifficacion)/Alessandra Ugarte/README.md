# Regresión lineal de NO₂ — District of Columbia, 2022

## 1. Descripción

Este proyecto analiza datos diarios de **dióxido de nitrógeno (NO₂)** registrados durante 2022 en el **District of Columbia, Estados Unidos**. Los datos fueron descargados de **EPA AirData / Air Quality System (AQS)**.

* **Periodo:** 01/01/2022 – 31/12/2022
* **Contaminante:** NO₂
* **Código AQS:** 42602
* **Unidad:** partes por mil millones (ppb)
* **Registros originales:** 1651
* **Estaciones:** 4
* **Observaciones diarias para la regresión:** 365

## 2. Objetivo

Realizar una regresión lineal para explorar la relación entre el **tiempo** y la **concentración máxima diaria promedio de NO₂** durante 2022.

## 3. Metodología

La variable independiente fue:

* **X:** número de día del año, donde 1 = 1 de enero y 365 = 31 de diciembre.

La variable dependiente fue:

* **Y:** promedio diario de la concentración máxima de NO₂ entre los monitores disponibles para esa fecha, expresado en ppb.

Se utilizó una regresión lineal de la forma:

**y = mx + b**

donde:

* **m** = pendiente
* **b** = intercepto
* **R²** = coeficiente de determinación

## 4. Resultados

La regresión obtenida para el promedio diario fue:

**y = -0.010740x + 23.477010**

con:

**R² = 0.013815**

La pendiente es de aproximadamente **-0.0107 ppb/día**. El R² obtenido es aproximadamente **0.0138**, por lo que la variable tiempo por sí sola explica una fracción pequeña de la variabilidad observada en las concentraciones diarias de NO₂.

### Regresión por estación

| Estación            |   n | Pendiente (ppb/día) | Intercepto (ppb) |       R² |
| ------------------- | --: | ------------------: | ---------------: | -------: |
| MCMILLAN NCore-PAMS | 730 |           -0.008068 |        20.499732 | 0.006148 |
| Near Road           | 365 |           -0.005342 |        28.108001 | 0.003351 |
| RIVER TERRACE       | 352 |           -0.019840 |        25.479871 | 0.043825 |
| Takoma Rec Center   | 204 |           -0.008898 |        25.071869 | 0.011087 |

## 5. Interpretación

La pendiente negativa de la regresión agregada indica que, en el modelo lineal simple utilizado, la concentración promedio diaria de NO₂ presenta una **tendencia descendente a lo largo de 2022**.

Sin embargo, el valor de R² es bajo (0.0138). Esto indica que una regresión lineal basada únicamente en el paso del tiempo no representa gran parte de la variabilidad diaria de NO₂.

La concentración puede variar por factores como meteorología, tráfico, emisiones locales, ubicación del monitor y variaciones estacionales.

Por ello, la regresión debe interpretarse como una descripción de la **tendencia lineal general del año**, y no como un modelo completo de las causas de la concentración de NO₂.

## 6. Visualización

El gráfico de la regresión lineal se encuentra en el archivo:

`regresion_NO2_DC_2022.png`

También puede visualizarse directamente en el repositorio.

## 7. Conclusiones

1. Se analizaron **1651 registros de NO₂** correspondientes a cuatro estaciones de monitoreo del District of Columbia durante 2022.
2. Para evitar duplicar fechas cuando había más de un monitor, se calculó un **promedio diario de la concentración máxima registrada**.
3. La regresión lineal obtenida fue **y = -0.010740x + 23.477010**.
4. El coeficiente de determinación fue **R² = 0.013815**, lo que muestra una relación lineal débil entre el número de día del año y la concentración diaria promedio.
5. El análisis permite identificar una tendencia lineal general, pero no explica por sí solo las variaciones diarias del NO₂.

## 8. Fuente de datos

**U.S. Environmental Protection Agency (EPA), AirData / Air Quality System (AQS).**

* [AirData](https://www.epa.gov/outdoor-air-quality-data)
* [Download Daily Data](https://www.epa.gov/outdoor-air-quality-data/download-daily-data)
* [AQS](https://aqs.epa.gov/aqsweb/)





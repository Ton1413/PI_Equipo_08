# Taller de Análisis y Modelado Predictivo — Consumo de Energía

## Descripción del Proyecto
En este taller se desarrolló un flujo completo de análisis de datos y **Machine Learning con Python** para estudiar el conjunto de datos `Data_PI_regresion.csv`. El objetivo principal fue analizar cómo variables operativas y ambientales (`Temperatura`, `Horas_Operacion`, `Carga` y `Humedad`) se relacionan con el **Consumo de Energía**, implementando modelos supervisados para predecir dicho consumo con alta precisión.

---

## 1. Exploración y Estadística Descriptiva
El conjunto de datos está compuesto por **5,000 registros y 5 variables numéricas continuas**. 
* Mediante la estadística descriptiva se establecieron los parámetros iniciales de las variables. Por ejemplo, la variable objetivo (`Consumo_Energia`) presentó una **media de 26.0** y una **desviación estándar de 5.6**, con valores que oscilan entre los 9.1 y 42.6.
* Esto permitió conocer la escala, distribución y dispersión de los datos antes de entrenar cualquier algoritmo predictivo.

---

## 2. Análisis de Correlación
A través de una matriz de correlación visualizada con un mapa de calor (`heatmap`), se evaluó el nivel de asociación lineal entre todas las variables cuantitativas. Este paso es fundamental para identificar de forma temprana qué factores operativos guardan una relación más estrecha con el consumo energético.

---

## 3. Modelado Predictivo: Regresión Lineal
Para anticipar el consumo de energía, los datos se dividieron en dos subconjuntos:
* **70% para entrenamiento** y **30% para prueba**, asegurando que el modelo se evaluara con datos totalmente nuevos.
* El modelo de **Regresión Lineal** ajustó los coeficientes para cada variable, destacando la importancia de factores como las *Horas de Operación* y la *Temperatura* en la ecuación de predicción.

---

## 4. Modelos Avanzados y Validación (Árboles de Decisión y OLS)
* **Árbol de Decisión (`DecisionTreeRegressor`):** Se implementó para capturar relaciones y comportamientos no lineales que los modelos estrictamente lineales no alcanzan a percibir, midiendo su rendimiento mediante el Error Cuadrático Medio (`MSE`).
* **Modelo Estadístico OLS (`statsmodels`):** Se utilizó para obtener un resumen estadístico formal (intervalos de confianza, estadísticos $t$, valores $p$ y $R^2$), validando la significancia matemática de las variables explicativas.

---

## Análisis de las Visualizaciones Clave del Proyecto

### 1. Matriz de Dispersión Cruzada (`pairplot`)
* **Qué representa:** Muestra de forma simultánea las relaciones bivariadas entre todas las variables del sistema (`Temperatura`, `Horas_Operacion`, `Carga`, `Humedad` y `Consumo_Energia`), ubicando los histogramas de distribución univariada en la diagonal principal.
* **Qué demuestra:** Permite identificar visualmente patrones clave, destacando una fuerte relación lineal ascendente entre las *Horas de Operación* y el *Consumo de Energía*, lo que justifica su peso como predictor principal en el modelo.
<img width="1024" height="1020" alt="WhatsApp Image 2026-09-15 at 11 26 59 PM" src="https://github.com/user-attachments/assets/b7b61536-b93d-4266-b2b7-5eb96aafee99" />



### 2. Consumo de Energía Real vs. Predicho
* **Qué representa:** Compara de manera directa los valores reales del consumo energético frente a las estimaciones calculadas por el modelo de regresión.
* **Qué demuestra:** Los puntos se agrupan de forma compacta a lo largo de una diagonal ascendente uniforme. Esto evidencia que **el modelo posee un alto nivel de precisión y una excelente capacidad de generalización** ante datos no vistos.
<img width="851" height="647" alt="WhatsApp Image 2026-09-15 at 11 26 48 PM" src="https://github.com/user-attachments/assets/eb89425d-e79b-4aaf-b02c-688fe9aa721e" />



### 3. Valores Residuales vs. Predichos
* **Qué representa:** Evalúa la distribución de los residuos (la diferencia entre el valor real y el estimado) frente a las predicciones del modelo.
* **Qué demuestra:** Los residuos se encuentran dispersos de manera aleatoria y homogénea alrededor de la línea de base cero, confirmando que **el modelo cumple con el supuesto de homocedasticidad** (es decir, el margen de error es estable y libre de sesgos sistemáticos).
<img width="854" height="687" alt="WhatsApp Image 2026-09-15 at 11 26 54 PM" src="https://github.com/user-attachments/assets/4a6eead0-249d-45d1-945d-7e1d54269d0e" />


---

## Resumen Metodológico

| Etapa del Proyecto | ¿Qué se hizo? | ¿Qué se demostró? |
| :--- | :--- | :--- |
| **Análisis Exploratorio** | Inspección y `pairplot` | Comportamiento, distribución y asociación cruzada de las variables. |
| **Modelado Predictivo** | Regresión Lineal & Árboles de Decisión | Capacidad de generalización y estimación matemática del consumo. |
| **Evaluación y Validación** | Gráficos de Real vs. Predicho & Residuos | Confiabilidad, precisión y estabilidad del error del modelo. |
| **Interpretación** | Importancia de características & OLS | Peso específico y significancia estadística de cada variable en el sistema. |

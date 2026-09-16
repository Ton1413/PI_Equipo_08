# Taller de IA con Python

## Descripción

En este taller se trabajó con **Machine Learning en Python** para analizar datos relacionados con el consumo de energía.

Las variables utilizadas fueron:

* Temperatura
* Horas de operación
* Carga
* Humedad
* Consumo de energía

Se utilizaron principalmente **Pandas, NumPy, Matplotlib, Seaborn y Scikit-learn**.

---

## Análisis de los datos

Primero se exploraron los datos para conocer su distribución y la relación entre las variables.

### Histogramas

Los histogramas permitieron observar cómo se distribuyen los valores de cada variable.


### Gráficos de densidad

La densidad ayudó a visualizar de una forma más clara la concentración de los datos.

### Matriz de correlación

Se utilizó una matriz de correlación para identificar qué variables presentan mayor relación entre sí.

```python
sns.heatmap(numeric_df1.corr(), annot=True, linewidths=2)
```

Este gráfico me pareció útil porque permite ver rápidamente las relaciones entre las variables antes de crear un modelo.

---

## Regresión lineal

Se entrenó un modelo de **regresión lineal** utilizando las variables de temperatura, horas de operación, carga y humedad para predecir el consumo de energía.

Los datos fueron divididos en:

* 70 % entrenamiento
* 30 % prueba

```python
lm = LinearRegression()
lm.fit(X_train, y_train)
predictions = lm.predict(X_test)
```

### Valores reales vs. predicción

Este gráfico permite comparar los valores reales con los valores que predijo el modelo. Mientras más cercanos estén los puntos entre sí, mejor se acerca la predicción al resultado real.

---

## Árbol de decisión

También se utilizó un **árbol de decisión para regresión**.

```python
tree_model = tree.DecisionTreeRegressor(
    max_depth=5,
    random_state=10
)

tree_model.fit(X_train, y_train)
```

Luego se realizaron predicciones y se utilizó el **Error Cuadrático Medio (MSE)** para evaluar el modelo.

### Importancia de las variables

Esta fue una de las partes que más me llamó la atención, ya que permite observar qué características tienen mayor participación dentro de las predicciones realizadas por el modelo.

---

## Gráficos más importantes

Los gráficos que considero más relevantes fueron:

### 1. Matriz de correlación

Permite observar la relación entre las variables.

### 2. Valores reales vs. predichos

Permite comprobar visualmente qué tan cerca están las predicciones de los valores reales.

### 3. Importancia de características

Permite identificar qué variables fueron más importantes para el árbol de decisión.

---

## Conclusión

En esta sesión pude entender mejor cómo se pasa de analizar un conjunto de datos a entrenar y evaluar un modelo de Machine Learning.

El proceso seguido fue:

**Datos → Visualización → Modelo → Predicción → Evaluación**

Lo que más destacaría es el uso de los gráficos, porque ayudan a entender los datos y también permiten interpretar mejor los resultados de los modelos.

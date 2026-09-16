# Taller de IA con Python

## Descripción

En este taller se trabajó el análisis de datos y los fundamentos de **Machine Learning utilizando Python**. Se utilizaron librerías como **NumPy, Pandas, Matplotlib, Seaborn y Scikit-learn** para analizar datos, realizar visualizaciones, entrenar modelos de regresión y evaluar sus predicciones.

El conjunto de datos utilizado contiene información relacionada con:

* Temperatura
* Horas de operación
* Carga
* Humedad
* Consumo de energía

El objetivo principal fue analizar cómo diferentes variables pueden relacionarse con el **consumo de energía** y posteriormente utilizar modelos de aprendizaje automático para realizar predicciones.

---

## Librerías utilizadas

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

También se utilizaron herramientas de **Scikit-learn** para construir y evaluar modelos:

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn import metrics
```

Además, se utilizaron:

```python
from sklearn.datasets import make_regression
from sklearn import tree
import statsmodels.api as sm
```

---

# 1. Carga y exploración de datos

Primero se cargó el archivo `Data_PI_regresion.csv` utilizando Pandas:

```python
df1 = pd.read_csv("Data_PI_regresion.csv")
```

Luego se utilizaron diferentes funciones para conocer la estructura y características del conjunto de datos:

```python
df1.head()
df1.info(verbose=True)
df1.describe().round(1)
df1.columns
```

El conjunto de datos contiene **5000 registros y 5 variables numéricas**:

| Variable          | Descripción            |
| ----------------- | ---------------------- |
| `Temperatura`     | Temperatura registrada |
| `Horas_Operacion` | Horas de operación     |
| `Carga`           | Nivel de carga         |
| `Humedad`         | Humedad registrada     |
| `Consumo_Energia` | Consumo de energía     |

Todas las variables se encuentran almacenadas como valores numéricos (`float64`).

---

# 2. Estadística descriptiva

Con:

```python
df1.describe().round(1)
```

se obtuvieron medidas estadísticas como:

* Cantidad de datos
* Media
* Desviación estándar
* Valor mínimo
* Percentiles
* Mediana
* Valor máximo

Por ejemplo, el consumo de energía presenta:

* Media: **26.0**
* Desviación estándar: **5.6**
* Mínimo: **9.1**
* Máximo: **42.6**

Esto permite tener una primera idea de cómo se distribuyen los datos antes de construir un modelo.

---

# 3. Visualización de datos

Se utilizaron herramientas de **Matplotlib y Seaborn** para representar gráficamente los datos.

Entre las visualizaciones trabajadas se encuentran:

* `pairplot`
* Histogramas
* Gráficos de densidad
* Matriz de correlación mediante `heatmap`
* Gráficos de dispersión
* Gráficos de valores reales frente a valores predichos
* Histogramas de residuos
* Gráficos de residuos frente a predicciones
* Gráficos de importancia de características

Estas visualizaciones permiten comprender mejor las relaciones existentes entre las variables.

---

# 4. Matriz de correlación

Se seleccionaron las variables numéricas:

```python
numeric_df1 = df1.select_dtypes(include=[np.number])
```

y posteriormente se calculó su correlación:

```python
numeric_df1.corr().round(4)
```

Para visualizarla se utilizó un mapa de calor:

```python
sns.heatmap(numeric_df1.corr(), annot=True, linewidths=2)
```

La matriz de correlación permite observar qué tan relacionadas están las variables entre sí.

---

# 5. Regresión lineal

Uno de los principales temas del taller fue la **regresión lineal**.

Se definieron las variables independientes `X` y la variable objetivo `y`:

```python
x = df1[column_list[0:len_feature-1]]
y = df1[column_list[len_feature-1]]
```

En este caso, las variables utilizadas para explicar el consumo fueron:

* Temperatura
* Horas de operación
* Carga
* Humedad

Mientras que la variable objetivo fue:

```text
Consumo_Energia
```

---

# 6. División de los datos

Para entrenar y evaluar el modelo se dividieron los datos en dos grupos:

```python
X_train, X_test, y_train, y_test = train_test_split(
    x, y, test_size=0.3, random_state=123
)
```

Se utilizó:

* **70 %** de los datos para entrenamiento.
* **30 %** de los datos para prueba.

Esto permite entrenar el modelo con una parte de los datos y posteriormente comprobar su comportamiento utilizando datos que no fueron utilizados durante el entrenamiento.

---

# 7. Entrenamiento del modelo

Se creó un modelo de regresión lineal:

```python
lm = LinearRegression()
```

y posteriormente se entrenó:

```python
lm.fit(X_train, y_train)
```

El modelo obtuvo un término de intersección de aproximadamente:

```text
2.741
```

Los coeficientes obtenidos fueron:

| Variable        | Coeficiente |
| --------------- | ----------: |
| Temperatura     |    0.137137 |
| Horas_Operacion |    1.668778 |
| Carga           |    0.096348 |
| Humedad         |    0.026791 |

Estos coeficientes forman parte del modelo utilizado para realizar las predicciones.

---

# 8. Predicciones

Después del entrenamiento se realizaron predicciones utilizando los datos de prueba:

```python
predictions = lm.predict(X_test)
```

De esta manera, el modelo genera un valor estimado de `Consumo_Energia` para cada registro del conjunto de prueba.

---

# 9. Evaluación mediante gráficos

Una parte importante del taller fue comparar los valores reales con los valores obtenidos por el modelo.

También se analizaron los residuos, que representan la diferencia entre los valores reales y los valores predichos.

Se utilizaron gráficos para:

* Comparar valores reales y predichos.
* Analizar la distribución de los residuos.
* Analizar los residuos respecto a los valores predichos.

Esto permite observar visualmente el comportamiento del modelo y detectar posibles errores o patrones.

---

# 10. Generación de datos para Machine Learning

Posteriormente se utilizaron datos artificiales mediante:

```python
from sklearn.datasets import make_regression
```

Se generaron:

* 100 muestras.
* 6 características.
* 3 características informativas.
* Ruido aleatorio.

```python
X, y, coef = make_regression(
    n_samples=100,
    n_features=6,
    n_informative=3,
    random_state=20,
    shuffle=False,
    noise=20,
    coef=True
)
```

Esto permitió trabajar con un conjunto de datos controlado para comprender mejor el funcionamiento de los modelos.

---

# 11. Árbol de decisión

En la segunda parte del taller se trabajó con un modelo de **árbol de decisión para regresión**:

```python
tree_model = tree.DecisionTreeRegressor(
    max_depth=5,
    random_state=10
)
```

El modelo fue entrenado con:

```python
tree_model.fit(X_train, y_train)
```

y posteriormente se realizaron predicciones:

```python
test_pred = tree_model.predict(X_test)
```

También se calculó el **Error Cuadrático Medio (MSE)**:

```python
metrics.mean_squared_error(y_test, test_pred)
```

El MSE permite medir la diferencia entre los valores reales y los valores predichos.

---

# 12. Importancia de las características

El árbol de decisión también permitió obtener la importancia relativa de las características:

```python
tree_model.feature_importances_
```

Esta información muestra cuánto contribuye cada característica al modelo.

Se representó mediante un gráfico de barras horizontales:

```python
plt.barh(
    range(n_features + 1, 1, -1),
    width=tree_model.feature_importances_,
    height=0.5
)
```

Esto ayuda a interpretar qué variables tienen mayor influencia dentro del modelo utilizado.

---

# 13. Modelo estadístico OLS

Finalmente, se utilizó `statsmodels` para crear un modelo de regresión mediante **OLS (Ordinary Least Squares)**:

```python
import statsmodels.api as sm

Xs = sm.add_constant(X)

stat_model = sm.OLS(y, Xs)

stat_result = stat_model.fit()

print(stat_result.summary())
```

El resumen generado muestra información estadística del modelo como:

* Coeficientes
* Error estándar
* Estadístico t
* `P>|t|`
* Intervalos de confianza
* R²
* AIC
* BIC
* Estadísticas de los residuos

---

# Gráficos importantes

## Gráfico 1 — Matriz de correlación

**Código utilizado:**

```python
sns.heatmap(numeric_df1.corr(), annot=True, linewidths=2)
```

### ¿Por qué es importante?

Este gráfico permite observar rápidamente la relación entre las variables del conjunto de datos.

Es importante porque antes de entrenar un modelo podemos analizar si existe alguna relación entre las variables independientes y `Consumo_Energia`.

<img width="775" height="618" alt="Captura de pantalla 2026-09-15 a la(s) 9 30 37 p  m" src="https://github.com/user-attachments/assets/22056f99-88ac-4e40-9077-dcde2fa7505d" />

---

## Gráfico 2 — Consumo de energía real vs. predicho

**Código utilizado:**

```python
plt.figure(figsize=(10,7))
plt.title("Consumo energia real vs Prediccion", fontsize=25)
plt.xlabel("Consumo energia real", fontsize=18)
plt.ylabel("Consumo energia predicho", fontsize=18)
plt.scatter(x=y_test, y=predictions)
```

### ¿Por qué es importante?

Este gráfico permite comparar directamente lo que ocurrió realmente en los datos con lo que predijo el modelo de regresión lineal.

Mientras más cercanos estén los puntos a una relación diagonal entre ambos valores, más parecidas son las predicciones a los valores reales.
<img width="869" height="674" alt="Captura de pantalla 2026-09-15 a la(s) 9 33 20 p  m" src="https://github.com/user-attachments/assets/67c7d155-117c-4674-b322-c905add14cbb" />

---

## Gráfico 3 — Importancia de las características

**Código utilizado:**

```python
plt.barh(
    range(n_features + 1, 1, -1),
    width=tree_model.feature_importances_,
    height=0.5
)
```

### ¿Por qué es importante?

Este gráfico permite interpretar el árbol de decisión mostrando la **importancia relativa de las características** utilizadas por el modelo.

Es útil porque no solamente interesa obtener una predicción, sino también comprender qué variables están siendo utilizadas por el modelo.

<img width="974" height="640" alt="Captura de pantalla 2026-09-15 a la(s) 9 35 06 p  m" src="https://github.com/user-attachments/assets/4ea91e05-9d9d-448e-9d00-314efdaa30c0" />

---

# 16. ¿Por qué elegí estos 3 gráficos?

Los tres gráficos muestran diferentes etapas del análisis:

| Gráfico                        | Etapa                     | ¿Qué demuestra?                    |
| ------------------------------ | ------------------------- | ---------------------------------- |
| Matriz de correlación          | Análisis exploratorio     | Relación entre las variables       |
| Real vs. Predicho              | Evaluación del modelo     | Comportamiento de las predicciones |
| Importancia de características | Interpretación del modelo | Participación de las variables     |

De esta manera, los gráficos cuentan una secuencia lógica:

**Datos → Relaciones → Modelo → Predicciones → Interpretación**

Esto permite explicar el taller de una manera más clara y demostrar que no solamente se realizaron gráficos, sino que las visualizaciones fueron utilizadas para comprender y evaluar los modelos de Machine Learning.

---

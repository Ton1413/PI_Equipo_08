# Taller de Introducción a la IA con Python — Regresión y Modelos Predictivos

## Sobre este trabajo

Este README documenta mi avance en el taller práctico de Python aplicado a Machine Learning, centrado en el análisis del consumo energético a partir de variables operativas (temperatura, horas de operación, carga y humedad). El objetivo fue recorrer todo el flujo de un proyecto de regresión: desde la exploración inicial de los datos hasta la construcción, evaluación e interpretación de distintos modelos predictivos.

## Herramientas empleadas

El desarrollo se apoyó en el stack estándar de análisis de datos en Python:

- **Pandas** y **NumPy** para la manipulación de los datos.
- **Matplotlib** y **Seaborn** para la parte gráfica.
- **Scikit-learn** para construir los modelos (regresión lineal y árbol de decisión) y calcular métricas de error.
- **Statsmodels** para obtener un resumen estadístico más detallado (OLS).

## El conjunto de datos

Trabajé con `Data_PI_regresion.csv`, un archivo con 5 columnas numéricas: `Temperatura`, `Horas_Operacion`, `Carga`, `Humedad` y `Consumo_Energia` (esta última como variable a predecir). Antes de modelar nada, revisé la estructura del dataset con `.info()` y `.describe()`, lo que me permitió confirmar que no había valores nulos y tener una primera noción de los rangos y la dispersión de cada variable.

## Exploración visual de los datos

Para entender cómo se comportaban las variables antes de entrenar cualquier modelo, generé:

- Un **`pairplot`** para ver de un vistazo las relaciones entre todas las variables a la vez.
<img width="822" height="657" alt="pairplot" src="https://github.com/user-attachments/assets/54c00ee5-bb1b-4672-87ac-9ff873fd5ea3" />

- Un **histograma** del consumo de energía (25 intervalos), que muestra cómo se reparte esta variable en todo el rango de valores.
<img width="926" height="516" alt="histograma" src="https://github.com/user-attachments/assets/65bc37e0-c2d8-4b94-ab33-0336e7ec954a" />

- Un **gráfico de densidad** del mismo consumo, que deja ver con más claridad que los datos se concentran principalmente entre 25 y 30.
<img width="751" height="722" alt="densidad" src="https://github.com/user-attachments/assets/e89a0e43-3f98-4504-97bf-cf5a9f9c19ca" />

- Un **mapa de calor de correlaciones**, que evidenció una relación lineal positiva marcada entre algunas variables predictoras y el consumo de energía.
<img width="892" height="762" alt="mapa de calor" src="https://github.com/user-attachments/assets/4b91d33d-c61c-412d-a57a-8a1a8da8e52a" />

## Modelo 1: Regresión lineal
Usé `Temperatura`, `Horas_Operacion`, `Carga` y `Humedad` como variables predictoras, y `Consumo_Energia` como variable objetivo. Separé el dataset en 70% para entrenamiento y 30% para prueba (`random_state=123`, para que el resultado sea reproducible) y entrené un modelo de `LinearRegression`.

Del modelo obtuve:

- Un intercepto de referencia.
- Los coeficientes de cada variable, guardados en un DataFrame junto con su error estándar y su estadístico t, lo que me permitió tener una primera idea de qué tan confiable era cada coeficiente.

Con las predicciones sobre el conjunto de prueba construí un gráfico de dispersión de **valores reales vs. predichos**: mientras más se acercan los puntos a una línea diagonal, mejor está prediciendo el modelo. También revisé los **residuos** (la diferencia entre lo real y lo predicho) mediante un histograma de densidad, para comprobar si se distribuían de forma aproximadamente normal, y un gráfico de residuos vs. predicciones, útil para detectar patrones que indicarían que el modelo se está equivocando de forma sistemática en cierto rango.

## Datos sintéticos para probar otro enfoque

Para no depender de un solo dataset, generé datos artificiales con `make_regression` (100 muestras, 6 variables, de las cuales solo 3 son realmente informativas, con ruido incluido). Esto me sirvió como un entorno controlado donde ya sé de antemano qué variables deberían pesar más, y así comprobar si el modelo lo detecta correctamente.

## Modelo 2: Árbol de decisión

Sobre esos datos sintéticos entrené un `DecisionTreeRegressor` (profundidad máxima de 5). Con las predicciones sobre el conjunto de prueba:

- Calculé el **Error Cuadrático Medio (MSE)**, que penaliza más fuerte los errores grandes al elevarlos al cuadrado.
- Grafiqué la **importancia relativa de cada característica** (`feature_importances_`) en un gráfico de barras horizontales, lo que permite ver de un vistazo qué variables está usando realmente el árbol para tomar sus decisiones, más allá de que hayan sido diseñadas como "informativas" o no.

*(Nota: en mi desarrollo trabajé específicamente con un árbol de decisión individual; no llegué a implementar un Random Forest, pero el flujo de evaluación —predicción, MSE e importancia de variables— es el mismo que se usaría para comparar ambos enfoques.)*

## Modelo 3: Regresión OLS con Statsmodels

Como cierre, ajusté un modelo de **Mínimos Cuadrados Ordinarios (OLS)** sobre los mismos datos sintéticos usando `statsmodels`. El resumen (`.summary()`) me dio una vista mucho más completa que la de scikit-learn: coeficientes, error estándar, estadístico t, p-valores, intervalos de confianza, R², AIC y BIC. Esto es útil porque no solo dice qué tan bien predice el modelo, sino también qué tan estadísticamente significativa es cada variable.

## Gráficos que elegí destacar y por qué

| Gráfico | Qué muestra | Por qué lo elegí |
|---|---|---|
| Histograma y densidad del consumo de energía | Cómo se distribuye la variable objetivo antes de modelar | Sin conocer la forma de la distribución, es difícil interpretar después si un error de predicción es "grande" o "pequeño" en contexto |
| Dispersión de valores reales vs. predichos (regresión lineal) | Qué tan bien el modelo lineal reproduce los valores reales | Es la forma más directa de ver el desempeño del modelo a simple vista, antes de mirar cualquier métrica numérica |
| Importancia de características (árbol de decisión) | Qué variables pesan más en las decisiones del árbol | Permite ir más allá de la predicción y entender el porqué del modelo, algo que la regresión lineal no muestra tan claramente |

En conjunto, estos tres gráficos siguen una secuencia que resume el trabajo: primero entender los datos, luego evaluar si el modelo predice bien, y finalmente interpretar qué está usando el modelo para hacerlo.

## Lo que más me quedó del taller

Lo que más resalto de esta práctica es la diferencia entre **predecir bien** e **interpretar bien**. La regresión lineal fue útil para tener una primera aproximación rápida y coeficientes fáciles de leer, pero el árbol de decisión y el análisis de importancia de variables me ayudaron a entender qué factores influían realmente, y el modelo OLS terminó de darle respaldo estadístico a esas conclusiones. Ver los tres enfoques trabajando sobre el mismo problema me dejó más claro que la elección de un modelo no depende solo de qué tan bajo es el error, sino también de qué tanto necesito explicar el resultado.

---
*Trabajo individual — Taller de IA con Python.*

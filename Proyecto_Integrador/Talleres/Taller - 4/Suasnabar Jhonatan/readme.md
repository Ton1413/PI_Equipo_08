# Redes Neuronales y Deep Learning

# 1. Concepto de CNN (Red Neuronal Convolucional)

Las Redes Neuronales Convolucionales (CNN) son un tipo especializado de red neuronal diseñado específicamente para procesar datos que tienen una estructura de cuadrícula, como las imágenes. A diferencia de las redes tradicionales, las CNN utilizan operaciones matemáticas llamadas "convoluciones" a través de filtros. Estos filtros recorren la imagen original para detectar patrones visuales como bordes, texturas y formas, de manera jerárquica. Gracias a esto, sumado a las capas de agrupación (pooling) que reducen el tamaño de los datos, las CNN son altamente eficientes para tareas de visión artificial y clasificación de imágenes.


## Etapas del Modelo Implementado y Código

- Preprocesamiento de datos: Carga, normalización y división del conjunto de datos en subconjuntos de entrenamiento y validación.

- Construcción de la arquitectura: Definición de las capas del modelo, integrando tanto capas convolucionales como capas densas.

- Entrenamiento y Regularización: Ajuste de los pesos de la red a través de múltiples épocas (epochs).

Código de la etapa de compilación y entrenamiento:


```
# Compilación del modelo
modelo_cnn.compile(optimizer='adam',
                   loss='binary_crossentropy',
                   metrics=['accuracy', 'AUC'])

# Entrenamiento del modelo
historia = modelo_cnn.fit(X_train, y_train, 
                          epochs=8, 
                          validation_data=(X_val, y_val))

```

Primero, se compila el modelo definiendo el optimizador (Adam, que ajusta los pesos eficientemente), la función de pérdida (al ser dos clases, se usa entropía cruzada binaria) y las métricas a evaluar (precisión y Área Bajo la Curva - AUC). Luego, se utiliza la función fit para iniciar el entrenamiento pasándole los datos de entrenamiento (X_train, y_train), el número de pasadas completas o épocas (epochs), y los datos de validación para evaluar el modelo al final de cada época sin que este los haya "visto" antes.

## Resultados:

Durante el entrenamiento y evaluación de nuestra "CNN desde cero", monitoreamos el comportamiento de la función de pérdida y las métricas de precisión.

## Análisis de Curvas de Aprendizaje:
En el primer gráfico observamos que la pérdida (Loss) de entrenamiento disminuye de manera constante desde la época 0 hasta la 7, lo que indica que el modelo efectivamente está aprendiendo y reduciendo sus errores sobre los datos de entrenamiento. Sin embargo, en el gráfico inferior de métricas de validación, notamos ciertas fluctuaciones. La precisión (Accuracy) finaliza alrededor del 63%, mientras que el ROC-AUC se mantiene relativamente estable cerca del 67-68%. Esto sugiere que, aunque el modelo aprende, todavía tiene cierta dificultad para generalizar perfectamente sobre datos nuevos.

<img width="676" height="621" alt="Captura de pantalla 2026-09-22 a la(s) 11 01 32 p  m" src="https://github.com/user-attachments/assets/b65a3704-bfe3-4865-9798-04f8fc4c0f3e" />

## Análisis de la Matriz de Confusión:
Al evaluar el modelo con un conjunto de datos balanceado de dos clases (0 y 1), la matriz de confusión revela los siguientes resultados:

Verdaderos Negativos (Clase 0): 42 predicciones correctas.

Verdaderos Positivos (Clase 1): 40 predicciones correctas.

Falsos Positivos: 34 veces el modelo predijo la clase 1 cuando en realidad era la 0.

Falsos Negativos: 33 veces el modelo predijo la clase 0 cuando en realidad era la 1.

Estos números son congruentes con el accuracy cercano al 60-65% observado en las curvas de validación. El modelo logra identificar correctamente poco más de la mitad de los casos para ambas clases, pero el alto número de falsos positivos y falsos negativos indica que las características que distinguen a la clase 0 de la clase 1 son complejas, sugiriendo que el modelo podría beneficiarse de más datos, más épocas de entrenamiento o una arquitectura ligeramente más profunda.

<img width="284" height="269" alt="Captura de pantalla 2026-09-22 a la(s) 11 02 44 p  m" src="https://github.com/user-attachments/assets/84161d17-b65a-42d6-bfb4-513cd8745acf" />

---------------------------------------------------

# 2. Implementación de Redes Neuronales con Keras

En esta práctica, nos enfocamos en el Procesamiento de Lenguaje Natural (NLP), utilizando la biblioteca Keras para construir un modelo capaz de analizar el sentimiento detrás de textos. El objetivo principal es clasificar reseñas de películas del conjunto de datos de IMDB en dos categorías: positivas o negativas (Clasificación Binaria).

## Etapas del Modelo Implementado y Código

Preprocesamiento de datos: Se descargó el dataset de IMDB limitando el vocabulario a las 10,000 palabras más frecuentes. Posteriormente, se aplicó una técnica de One-Hot Encoding mediante una función personalizada (vectorizar) para transformar las secuencias de enteros en tensores de ceros y unos, preparándolos para ingresar a la red neuronal.

Construcción de la arquitectura base: Se construyó un modelo Sequential original con dos capas ocultas densas (Dense) de 16 neuronas utilizando la función de activación ReLU, y una capa de salida de 1 neurona con activación Sigmoide para entregar una probabilidad (0 a 1).

Técnicas contra el Sobreajuste (Overfitting): Al evaluar el modelo original, se detectó que memorizaba los datos de entrenamiento. Para solucionarlo, se experimentó con tres enfoques:

Reducir la capacidad de la red (usando 4 neuronas).

Aplicar Regularización L2 (penalizando pesos grandes).

Aplicar Dropout: Desactivando aleatoriamente el 50% de las neuronas durante el entrenamiento.

Código implementado para la versión final del modelo usando Dropout:

```
# Creación del modelo con capas de Dropout
model4 = models.Sequential()
model4.add(layers.Dense(16, activation='relu', input_shape=(10000,)))
model4.add(layers.Dropout(0.5)) # Desactiva el 50% de neuronas
model4.add(layers.Dense(16, activation='relu'))
model4.add(layers.Dropout(0.5)) # Desactiva el 50% de neuronas
model4.add(layers.Dense(1, activation='sigmoid'))

# Compilación del modelo
model4.compile(optimizer='rmsprop',
               loss='binary_crossentropy',
               metrics=['accuracy'])

# Entrenamiento
modelb4 = model4.fit(partial_x_train,
                     partial_y_train,
                     epochs=20,
                     batch_size=512,
                     validation_data=(x_val,y_val))

```

Este bloque define nuestra red neuronal más robusta. Al agregar layers.Dropout(0.5) después de cada capa densa, obligamos a la red a aprender patrones usando diferentes combinaciones de neuronas en cada paso, evitando que dependa demasiado de características específicas. Finalmente, se compila usando el optimizador rmsprop y la función de pérdida binary_crossentropy (ideal para dos clases).

## Resultados :

Durante el proceso, analizamos el comportamiento del error (Loss) para entender cómo generalizaba nuestro modelo.

1. El problema del Sobreajuste (Modelo Original)

<img width="654" height="646" alt="Captura de pantalla 2026-09-22 a la(s) 11 30 19 p  m" src="https://github.com/user-attachments/assets/b088c0c9-5856-47ea-af4b-8a8e4fad1f71" />

En la gráfica observamos que el error de entrenamiento (línea azul) disminuye constantemente. Sin embargo, el error de validación (línea naranja punteada) comienza a subir a partir de la época 4 o 5. Esto es un claro ejemplo de sobreajuste: la red está aprendiendo demasiado bien los datos de entrenamiento y pierde su capacidad para predecir datos nuevos.

2. Solución aplicando Dropout

<img width="656" height="643" alt="Captura de pantalla 2026-09-22 a la(s) 11 32 03 p  m" src="https://github.com/user-attachments/assets/8b80b18e-cdb1-47a4-92dd-3792d236c09a" />

Al comparar el error de validación del modelo con Dropout (línea azul sólida) frente al modelo original (línea naranja punteada), notamos una mejora. El modelo con Dropout retrasa la aparición del sobreajuste, manteniendo el error de validación más bajo y estable durante más épocas, lo que demuestra que la técnica obligó a la red a generalizar mejor.

3. Predicciones Finales

<img width="403" height="221" alt="Captura de pantalla 2026-09-22 a la(s) 11 33 02 p  m" src="https://github.com/user-attachments/assets/6fc6d0c2-4c29-410a-bedb-3e70d5fae107" />

Al evaluar el índice 10 de nuestro conjunto de pruebas, el modelo arrojó un valor de 0.9879335. Como nuestra capa de salida usa una función sigmoide, esto se traduce en que la red neuronal tiene un 98.7% de seguridad de que esa reseña es positiva, demostrando que el modelo ha aprendido exitosamente a analizar el sentimiento de los textos.

---------------------------------------------------

# 3. Perceptrón

Las redes neuronales artificiales nos permiten resolver problemas complejos mediante el aprendizaje automático, inspirándose en el funcionamiento del cerebro humano. En esta práctica, exploramos desde la unidad matemática más básica (el Perceptrón) hasta la construcción de una red neuronal profunda utilizando Keras para el Procesamiento de Lenguaje Natural (NLP).

## Fundamentos

Antes de utilizar bibliotecas avanzadas, construimos un Perceptrón desde cero para entender su funcionamiento interno. Un perceptrón es un modelo sencillo de inteligencia artificial que recibe datos de entrada ($X$), los multiplica por ciertos pesos ($W$), suma un sesgo o bias ($b$) y pasa el resultado por una función de activación para producir una predicción.

Fórmula matemática base:      $$Salida = f(W \cdot X + b)$$

## Implementación en Código:

Definimos la lógica del perceptrón y funciones de activación básicas (escalón y tangente hiperbólica) en Python puro usando numpy:

```
import numpy as np

# Función de activación escalón (devuelve 0 o 1)
def step_function(x):
    return 1 if x >= 0 else 0

# Función del perceptrón
def perceptron(inputs, weights, bias, activation_func):
    # Calcular la suma ponderada de las entradas
    weighted_sum = np.dot(inputs, weights) + bias
    # Aplicar la función de activación
    output = activation_func(weighted_sum)
    return output

```

## Aplicaciones Prácticas y Límites de Decisión:

Para probar nuestro perceptrón, simulamos compuertas lógicas (AND y OR). Descubrimos que el perceptrón actúa como un clasificador lineal, trazando una línea recta (Frontera o Límite de Decisión) para separar los resultados positivos de los negativos.

<img width="398" height="399" alt="Captura de pantalla 2026-09-23 a la(s) 12 10 30 a  m" src="https://github.com/user-attachments/assets/cf0247c0-5622-4da7-879c-0d05f50c6206" />

La gráfica muestra cómo un solo perceptrón puede separar exitosamente los puntos de una compuerta OR (línea roja) y una compuerta AND (línea verde) trazando una única línea recta.

### El Problema del XOR:
Al intentar resolver la compuerta lógica XOR (donde la salida es 1 solo si las entradas son diferentes), un solo perceptrón fracasa. Los puntos no pueden separarse con una sola línea recta.

<img width="398" height="400" alt="Captura de pantalla 2026-09-23 a la(s) 12 11 14 a  m" src="https://github.com/user-attachments/assets/1704644c-fbeb-41e2-8a12-893d087b6508" />

Como se ve en la gráfica, para separar el problema XOR necesitamos dos líneas azules. Esto demuestra una regla fundamental del Deep Learning: Un perceptrón no puede resolver problemas no lineales como el XOR, pero una red de múltiples perceptrones (capas ocultas) sí puede hacerlo.

---------------------------------------------------

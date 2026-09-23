# Introducción al Aprendizaje Profundo: Entrenamiento, Perceptrón y Separabilidad

En el estudio del Deep Learning y las redes neuronales artificiales, resulta fundamental comprender tanto el comportamiento dinámico del proceso de aprendizaje como las limitaciones matemáticas de los modelos más simples. A través del análisis de las curvas de pérdida y las fronteras de decisión, es posible diagnosticar el rendimiento de una red y entender por qué arquitecturas más complejas se volvieron indispensables para resolver problemas del mundo real.
# Perceptrón y Redes Neuronales Convolucionales (CNN)

## 1. El Perceptrón (La base)

El perceptrón es la unidad fundamental de las redes neuronales artificiales. Simula el funcionamiento de una neurona biológica:

- **Entradas ($x$):** Datos que recibe la neurona.
- **Pesos ($w$) y Sesgo ($b$):** Parámetros que la red ajusta durante el entrenamiento para dar más o menos importancia a ciertas entradas.
- **Función de Activación:** Decide si la neurona debe "activarse" o no, introduciendo no linealidad al modelo.

## 2. Redes Neuronales Convolucionales (CNN)

Las **Convolutional Neural Networks (CNN)** están diseñadas específicamente para el análisis de imágenes mediante el estudio de píxeles cercanos. Utilizan pequeños filtros llamados **kernels** que recorren la imagen buscando patrones específicos.

### Componentes Clave:

**Convolución (Conv2D):**

Aplica filtros (kernels) para obtener mapas de características.

- Aprende a detectar bordes, texturas y estructuras más complejas en las capas profundas.

**Activación (ReLU):**

Introduce no linealidad.

Su función es:

$$
\text{ReLU}(x) = \max(0, x)
$$

Si el valor es negativo se anula ($0$), y si es positivo se mantiene igual.

**Pooling (MaxPool):**

Reduce la resolución espacial manteniendo la información más importante.

- Ayuda a la generalización del modelo y disminuye el costo computacional.

**Capas Fully-Connected (Densas):**

Integran toda la información extraída previamente para realizar la tarea final de clasificación o regresión.

### Ejemplo visual del flujo en una CNN:

*(Representación conceptual del recorrido de la imagen)*

```text
[Imagen de Entrada] ---> [Conv2D + ReLU] ---> [MaxPool] ---> [Capas Densas] ---> [Resultado]
```
## 3. Implementación en Keras (Código base)

Keras es una API de alto nivel para construir y entrenar redes neuronales en Python de forma sencilla. A continuación, se muestra cómo estructurar un modelo secuencial utilizando capas convolucionales (CNN):

```python
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Activation

# 1. Crear el modelo secuencial
model = Sequential()

# 2. Capa Convolucional y Pooling
model.add(Conv2D(32, (3, 3), input_shape=(64, 64, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))

# 3. Segunda Capa Convolucional
model.add(Conv2D(64, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))

# 4. Aplanar (Flatten) para conectar con capas densas
model.add(Flatten())

# 5. Capas totalmente conectadas (Fully-Connected)
model.add(Dense(128))
model.add(Activation('relu'))

# Capa de salida (ejemplo para clasificación binaria)
model.add(Dense(1))
model.add(Activation('sigmoid'))

# Compilación del modelo
model.compile(optimizer='adam', 
              loss='binary_crossentropy', 
              metrics=['accuracy'])

model.summary()
```
## 4. Integración y Flujo de Trabajo

La integración de estos conceptos en un proyecto de Deep Learning sigue generalmente los siguientes pasos:

**Preprocesamiento:** Las imágenes se normalizan y se ajustan a un tamaño estándar (ej. $64 \times 64$ píxeles).

**Extracción de Características (CNN):** Las capas Conv2D y MaxPool se encargan de escanear la imagen en busca de patrones locales (líneas, formas, objetos).

**Clasificación (Capas Densas):** Los mapas de características se aplanan y se pasan a las neuronas tradicionales para emitir una predicción final (por ejemplo, clasificar si la imagen contiene o no un objeto determinado).

## 5. Monitoreo del Entrenamiento y Diagnóstico de Overfitting

La gráfica de rendimiento (loss) en función de las épocas es una de las herramientas de diagnóstico más potentes durante el desarrollo de modelos de aprendizaje automático.

**Comportamiento del Conjunto de Entrenamiento (Línea Azul):** Muestra cómo el error disminuye constantemente conforme avanza el entrenamiento. El modelo reduce la pérdida ajustando sus parámetros internos iterativamente.

**Comportamiento del Conjunto de Validación (Línea Naranja Discontinua):** Al principio disminuye de forma saludable, pero a partir de cierto punto comienza a incrementarse de manera sostenida. Este fenómeno es un indicador clásico de sobreajuste (overfitting), lo que significa que el modelo ha dejado de aprender patrones generales y ha empezado a memorizar los datos específicos de entrenamiento.

<img width="826" height="813" alt="a1936109-b822-4710-af73-f1083a5800be" src="https://github.com/user-attachments/assets/c7a892f0-11da-47a6-b324-6512a22b6b66" />


Fragmento de código para configurar el entrenamiento con validación:

```python
# Entrenamiento de la red controlando los datos de validación por época
history = model.fit(
    X_train, y_train, 
    epochs=20, 
    batch_size=32,
    validation_data=(X_val, y_val)
)
```
## 6. El Límite del Perceptrón Simple y el Desafío del XOR

La representación visual de las compuertas lógicas nos permite observar directamente la capacidad geométrica de un perceptrón simple.

**Funciones Linealmente Separables (AND / OR):** Las operaciones lógicas básicas pueden resolverse trazando una única línea recta (un hiperplano) que separe los resultados verdaderos de los falsos. El perceptrón ajusta sus pesos y sesgos para encontrar dicha pendiente.

**El Problema del XOR:** La función lógica XOR (O exclusivo) arroja verdadero solo cuando las entradas son distintas. Al graficar sus puntos $(0,0)$, $(1,1)$, $(0,1)$ y $(1,0)$, resulta geométricamente imposible trazar una sola línea recta que separe los grupos correctamente. Esto demostró históricamente la limitación del perceptrón de una sola capa y motivó la creación de redes multicapa con capas ocultas y funciones de activación no lineales.

<img width="503" height="505" alt="756be402-6fd4-41f7-b738-fb257feadb77" src="https://github.com/user-attachments/assets/f2d6e00e-1ce0-44e5-933c-51ce7a4232c6" />


Fragmento de código para un Perceptrón o Capa Densa:

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

# Inicialización de un modelo secuencial básico
model = Sequential()

# Capa densa con 1 neurona de salida y función de activación sigmoide
model.add(Dense(1, input_shape=(2,), activation='sigmoid'))
```
## 7. Resumen General
7.1. El Perceptrón y la Separabilidad Lineal (AND, OR y XOR)

- ¿Qué es?: Es la unidad de cálculo fundamental de una red neuronal, emulando una neurona biológica mediante entradas, pesos, un sesgo y una función de activación.

- Compuertas Lógicas: Un perceptrón simple puede resolver funciones linealmente separables como AND y OR trazando una línea recta en un plano cartesiano.

- La limitación del XOR: Funciones no lineales como XOR no pueden dividirse con una sola línea recta, lo que demostró la necesidad de construir redes neuronales multicapa (más profundas) para resolver problemas complejos.

7.2. Dinámica del Entrenamiento (Curvas de Aprendizaje)

- Pérdida (Loss) en Entrenamiento vs. Validación: Al entrenar un modelo a lo largo de las épocas, la gráfica de entrenamiento muestra cómo el error disminuye continuamente.

- Sobreajuste (Overfitting): Si la curva de validación comienza a subir mientras la de entrenamiento sigue bajando, indica que el modelo está memorizando los datos en lugar de generalizar, sirviendo como señal clave para detener el entrenamiento a tiempo.

7.3. Redes Neuronales Convolucionales (CNN) en Keras

- Estructura Modular: Las CNN están optimizadas para procesar imágenes mediante capas especializadas:

- Convolución (Conv2D): Aplica filtros (kernels) para extraer patrones espaciales y características locales (bordes, formas).

- Activación (ReLU): Introduce no linealidad transformando valores negativos en cero.

- Pooling (MaxPool): Reduce la dimensionalidad espacial para disminuir el costo computacional y retener lo más importante.

- Aplanado y Capas Densas (Flatten y Dense): Convierten las características extraídas en vectores planos para que las neuronas tradicionales realicen la clasificación final (por ejemplo, mediante una salida sigmoidea).

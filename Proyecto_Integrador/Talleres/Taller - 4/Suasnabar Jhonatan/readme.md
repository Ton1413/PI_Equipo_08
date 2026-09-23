# Introducción a las Redes Neuronales y Deep Learning

## 1. El Perceptrón: Los Fundamentos

El perceptrón es la unidad básica de una red neuronal, inspirado en el funcionamiento de una neurona biológica. Es un algoritmo para el aprendizaje supervisado de clasificadores binarios.

**Conceptos clave aprendidos:**

* Pesos (weights) y sesgo (bias).

* Funciones de activación (ej. escalón).

* Limitación principal: Solo puede resolver problemas linealmente separables (como compuertas AND/OR, pero no XOR).

### Gráfico importante :


<img width="401" height="401" alt="Captura de pantalla 2026-09-22 a la(s) 7 05 15 p  m" src="https://github.com/user-attachments/assets/d363e9b2-6f3d-4fff-a39f-1f460fc14b0c" />

<img width="396" height="402" alt="Captura de pantalla 2026-09-22 a la(s) 7 05 33 p  m" src="https://github.com/user-attachments/assets/bf7fd370-0603-4c23-90de-71ac0dffe588" />


### Código (Perceptrón básico)

```
from sklearn.linear_model import Perceptron
import numpy as np

# Datos de ejemplo (Compuerta AND)
X = np.array([[0, 0], [0, 1], [1, 0], [1, 1]])
y = np.array([0, 0, 0, 1])

# Inicializar y entrenar el modelo
modelo_perceptron = Perceptron(max_iter=1000, eta0=0.1)
modelo_perceptron.fit(X, y)

print("Predicciones:", modelo_perceptron.predict(X))

```

## 2. Implementación de Redes Neuronales con Keras

Keras es una API de alto nivel que facilita la creación y entrenamiento de modelos de Deep Learning (usualmente sobre TensorFlow). Aprendimos a crear redes neuronales multicapa (Multilayer Perceptron o Redes Densas).

**Conceptos clave aprendidos:**

* El modelo `Sequential`.

* Capas `Dense` (Totalmente conectadas).

* Funciones de activación modernas: `ReLU` (capas ocultas) y `Softmax` o `Sigmoid` (capa de salida).

* Compilación del modelo (Optimizador `Adam`, función de pérdida `categorical_crossentropy`).

### Gráfico importante :

<img width="653" height="643" alt="Captura de pantalla 2026-09-22 a la(s) 7 08 19 p  m" src="https://github.com/user-attachments/assets/14757390-b033-435b-9b16-3db7f559b3d2" />

<img width="652" height="650" alt="Captura de pantalla 2026-09-22 a la(s) 7 08 31 p  m" src="https://github.com/user-attachments/assets/2f38b272-a461-4920-b022-5e9f40f53f15" />

<img width="680" height="616" alt="Captura de pantalla 2026-09-22 a la(s) 7 10 35 p  m" src="https://github.com/user-attachments/assets/198bfbc7-8b11-4d40-abc7-7f998d3ea76a" />


### Código (Red Densa con Keras)

```
from tensorflow import keras
from tensorflow.keras import layers

# Construcción del modelo Secuencial
modelo_denso = keras.Sequential([
    layers.Dense(64, activation='relu', input_shape=(784,)), # Capa de entrada + oculta
    layers.Dense(32, activation='relu'),                     # Capa oculta
    layers.Dense(10, activation='softmax')                   # Capa de salida (10 clases)
])

# Compilación
modelo_denso.compile(optimizer='adam',
                     loss='sparse_categorical_crossentropy',
                     metrics=['accuracy'])

# Resumen de la arquitectura
modelo_denso.summary()

# (El entrenamiento se haría con modelo_denso.fit(X_train, y_train, epochs=10))

```

## 3. Redes Neuronales Convolucionales (CNN)

Las CNN son arquitecturas especializadas en procesar datos con topología de cuadrícula, como imágenes. Son la base de la visión por computadora moderna.

**Conceptos clave aprendidos:**

* **Convolución (`Conv2D`):** Aplica filtros para extraer características espaciales (bordes, texturas).

* **Pooling (`MaxPooling2D`):** Reduce la dimensionalidad de la imagen, conservando la información más importante y reduciendo el costo computacional.

* **Flatten:** Aplana los mapas de características 2D a un vector 1D para conectarlos a una red densa final para la clasificación.

### Gráficos importantes :


<img width="284" height="274" alt="Captura de pantalla 2026-09-22 a la(s) 7 01 04 p  m" src="https://github.com/user-attachments/assets/df9a4ead-53fc-48ad-8f30-2bce83b7879f" />

<img width="791" height="283" alt="Captura de pantalla 2026-09-22 a la(s) 7 01 31 p  m" src="https://github.com/user-attachments/assets/7cd15909-9993-4f54-8a49-6b58b171be40" />

<img width="396" height="404" alt="Captura de pantalla 2026-09-22 a la(s) 7 04 56 p  m" src="https://github.com/user-attachments/assets/99af9740-91f0-4ce5-bb06-30c1174f04d6" />


### Código (Arquitectura CNN)

```
# Construcción de una CNN clásica para clasificación de imágenes
modelo_cnn = keras.Sequential([
    # Bloque Convolucional 1
    layers.Conv2D(32, (3, 3), activation='relu', input_shape=(28, 28, 1)),
    layers.MaxPooling2D((2, 2)),
    
    # Bloque Convolucional 2
    layers.Conv2D(64, (3, 3), activation='relu'),
    layers.MaxPooling2D((2, 2)),
    
    # Clasificador Final (Red Densa)
    layers.Flatten(),
    layers.Dense(64, activation='relu'),
    layers.Dense(10, activation='softmax') # Ejemplo para 10 clases (ej. MNIST)
])

modelo_cnn.compile(optimizer='adam',
                   loss='sparse_categorical_crossentropy',
                   metrics=['accuracy'])

```

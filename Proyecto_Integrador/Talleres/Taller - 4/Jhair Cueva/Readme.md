# Redes Neuronales

## Introducción

En esta práctica aprendimos conceptos básicos sobre **CNN, Keras y perceptrón**. También vimos cómo se construyen estos modelos, cómo interpretamos sus resultados y qué utilidad podrían tener dentro de nuestro proyecto **Reconexa**.

---

## 1. Redes Neuronales Convolucionales (CNN)

Las **CNN** son redes neuronales utilizadas principalmente para trabajar con imágenes. Estas redes pueden detectar características como bordes, formas y texturas.

En la práctica se trabajó con imágenes de **vidrio y plástico**.

### Código importante

```python
class SimpleCNN(nn.Module):
    def __init__(self, num_classes):
        super().__init__()

        self.features = nn.Sequential(
            nn.Conv2d(1, 16, kernel_size=3, padding=1),
            nn.ReLU(),
            nn.MaxPool2d(2),

            nn.Conv2d(16, 32, kernel_size=3, padding=1),
            nn.ReLU(),
            nn.MaxPool2d(2)
        )
```

<img width="502" height="562" alt="image" src="https://github.com/user-attachments/assets/8c5cfe77-1f87-43e7-ac75-4a27a3e5cb65" />

### Explicación

`Conv2d` aplica filtros sobre la imagen para encontrar características importantes.
`ReLU` permite que la red pueda aprender relaciones más complejas y `MaxPool2d` reduce el tamaño de la información procesada.

### Resultado

<img width="867" height="423" alt="image" src="https://github.com/user-attachments/assets/696ffccf-1ad7-40ec-a3db-9aac43c605af" />


El gráfico permite observar cómo cambia el rendimiento del modelo durante el entrenamiento. Una mejora en las métricas indica que la red está aprendiendo a diferenciar las imágenes.

---

## 2. Keras

**Keras** es una herramienta que permite construir y entrenar redes neuronales de una manera más sencilla.

### Código importante

```python
model = models.Sequential()

model.add(
    layers.Dense(
        16,
        activation='relu',
        input_shape=(10000,)
    )
)

model.add(
    layers.Dense(
        16,
        activation='relu'
    )
)

model.add(
    layers.Dense(
        1,
        activation='sigmoid'
    )
)
```

<img width="1407" height="210" alt="image" src="https://github.com/user-attachments/assets/6a77ddd9-910b-4b1c-a0bf-9273f11e06f9" />



### Explicación

`Sequential` permite agregar las capas una después de otra.

Las capas `Dense` contienen neuronas totalmente conectadas. Las dos primeras utilizan `ReLU`, mientras que la última utiliza `sigmoid`, que entrega un valor entre 0 y 1 para realizar una clasificación binaria.

### Gráfico

<img width="826" height="813" alt="image" src="https://github.com/user-attachments/assets/1272c07a-70bb-4ca2-8f25-c2592ec1ffb9" />


Este gráfico permite comparar el error de entrenamiento con el error de validación y observar si el modelo está aprendiendo correctamente o empieza a presentar sobreajuste.

---

## 3. Perceptrón

El **perceptrón** es una de las formas más básicas de una neurona artificial. Trabaja utilizando entradas, pesos, un `bias` y una función de activación.

### Código importante

```python
def perceptron(inputs, weights, bias, activation_func):

    weighted_sum = np.dot(
        inputs,
        weights
    ) + bias

    output = activation_func(
        weighted_sum
    )

    return output
```
<img width="642" height="378" alt="image" src="https://github.com/user-attachments/assets/9ac124c0-49e6-4daa-a5f3-7e095d80053d" />

<img width="503" height="505" alt="image" src="https://github.com/user-attachments/assets/f7ab9cd8-406a-4c64-a5db-2b253ab34a53" />

### Explicación

`np.dot()` multiplica las entradas por sus respectivos pesos.

Luego se suma el `bias` y el resultado pasa por una función de activación para obtener la salida final.

### Gráfico
<img width="503" height="505" alt="image" src="https://github.com/user-attachments/assets/0d568019-5cf7-429b-9810-47a61170c92f" />



La gráfica muestra cómo el perceptrón crea una frontera para separar diferentes grupos de datos.

---

## 4. Aplicación en Reconexa

De los temas estudiados, considero que las **CNN** serían las más útiles para nuestro proyecto.

Reconexa utiliza una cámara para registrar imágenes de los residuos. Estas imágenes podrían ser enviadas a una CNN para intentar reconocer el tipo de material.

```text
Residuo
   ↓
Cámara
   ↓
Imagen
   ↓
CNN
   ↓
Clasificación del material
```

La clasificación también podría complementarse con los datos obtenidos mediante los sensores de **peso y volumen**.

---

## Conclusión

En esta práctica comprendimos que el **perceptrón** representa la base del funcionamiento de una neurona artificial, **Keras** facilita la creación de redes neuronales y las **CNN** permiten trabajar principalmente con imágenes.

Para Reconexa, una CNN podría ser utilizada para ayudar a identificar diferentes tipos de residuos mediante las imágenes capturadas por la cámara.


# TAREA: CNN, Keras y perceptrón

## Introducción
Se realizaron tres ejercicios vinculados a redes neuronales en el cuaderno. Primero, se clasificaron imágenes de plástico y vidrio utilizando redes neuronales convolucionales (CNN) implementadas en PyTorch. También se utilizó Keras para clasificar como positivas o negativas y finalmente, se probó un perceptrón con valores establecidos manualmente.

## 1. CNN
### Concepto
Una red neuronal convolucional (CNN) fue creada para procesar imágenes manteniendo el vínculo espacial entre píxeles próximos. Por ejemplo, se emplea para detectar rasgos visuales o para la clasificación de imágenes. Sus filtros recorren la imagen y generan mapas de características; en las capas iniciales pueden detectar patrones sencillos, pero en las siguientes son capaces de fusionar señales más complejas. Durante la capacitación, el modelo adquiere los valores de esos filtros.

Se emplea el conjunto TrashNet, que está restringido a dos clases: `glass` (vidrio, etiqueta 0) y `plastic` (plástico, etiqueta 1). La clase `TrashDataset` distribuye las imágenes en tres categorías: entrenamiento, validación y prueba. Las entradas de la CNN básica son tensores que tienen un canal y miden 384 × 512 píxeles, mientras que la salida refleja dos categorías.

<img width="900" height="597" alt="vidrios" src="https://github.com/user-attachments/assets/73cbe972-0f38-4ccc-b9ff-cad689e7c9d7" />

### Etapas del modelo implementado
La clase `SimpleCNN` tiene tres capas de convolución, llamadas `Conv2d`. La primera convierte un canal en dieciséis, la segunda de dieciséis a treinta y dos y la tercera de treinta y dos a sesenta y cuatro; emplean filtros 3 × 3. Cada convolución es seguida por `ReLU`, que permite aprender correlaciones no lineales. Se ejecuta `MaxPool2d(2)` tras las dos primeras, lo cual disminuye la mitad las dimensiones en términos espaciales y retiene las respuestas más sobresalientes de cada área. `AdaptiveAvgPool2d((1, 1))` condensa cada uno de los 64 mapas finales en un solo valor. Finalmente, `Flatten` dispone esos valores y `Linear(64, 2)` crea las dos salidas de clasificación.

Se emplea `Adam` con una tasa de aprendizaje de `lr=1e-3` para modificar los parámetros y se recurre a `CrossEntropyLoss` como función de pérdida. `batch_size=128` señala el número de imágenes que se procesan en cada lote. Las `epochs` son las pasadas totales por el conjunto de entrenamiento; en el caso de la CNN inicial, se realizan ocho, y en el caso de la versión con aumento de datos, seis. El código utiliza `cuda` si Colab tiene GPU; de lo contrario, recurre a CPU.

Se registran `train_loss` (error durante el entrenamiento), `val_acc` (proporción de aciertos en validación) y `val_auc` (ROC-AUC, indicador de qué tan bien se ordenan las imágenes de una clase frente a la otra según la puntuación del modelo). Para calcular las predicciones, `softmax` transforma las dos salidas en probabilidades y se aplica un umbral de 0,5 a la probabilidad de plástico. La matriz de confusión permite observar aciertos y errores por clase, algo que la exactitud total no muestra por sí sola.

### Resultados de la CNN
La CNN entrenada desde cero obtuvo **55,70 % de exactitud** y **0,6328 de ROC-AUC** en prueba. Su pérdida de entrenamiento disminuyó entre las épocas, pero la exactitud de validación fluctuó; por eso, una pérdida menor no significa que ya clasifique bien imágenes nuevas. En la matriz de confusión acertó 66 de 76 imágenes de vidrio y 17 de 73 de plástico. Clasificó 56 imágenes de plástico como vidrio, su error más frecuente.

<img width="852" height="485" alt="matriz" src="https://github.com/user-attachments/assets/527e15fd-629c-4e4e-bf9c-e6bc2b800d74" />

Luego se aplicó aumento de datos al entrenamiento mediante rotaciones de hasta 10° y traslaciones suaves. Esta versión alcanzó **63,09 % de exactitud** y **0,6361 de ROC-AUC** en prueba. La exactitud mejoró frente al modelo inicial, mientras que el ROC-AUC cambió poco.

Por último, se utilizó **ResNet18**, una red neuronal convolucional preentrenada, para realizar aprendizaje por transferencia. Se ajustaron las imágenes a 224 × 224 y se replicó su único canal hasta conseguir tres canales para la entrada del modelo. En primer lugar, se entrenó la capa más reciente y posteriormente se ajustaron las capas finales de extracción de características. En la prueba, alcanzó una precisión de **87,92 %** y un ROC-AUC de **0,9634**. Su matriz de confusión muestra 73 aciertos en el vidrio y 58 en el plástico, además de que se confundieron tres imágenes de vidrio con plástico y quince de plástico con vidrio. Es el mejor rendimiento registrado entre los tres modelos de imágenes del cuaderno.

<img width="867" height="422" alt="modelos en prueba" src="https://github.com/user-attachments/assets/43dd6e03-a9a6-4f70-9346-2df0f1a70c7b" />

El notebook incluye un ejemplo de **Grad-CAM**, que resalta zonas influyentes en una predicción de ResNet18. Ayuda a observar dónde se concentró el modelo en esa imagen específica, pero no demuestra por sí solo que todas las predicciones sean correctas.

<img width="1027" height="482" alt="superpsic1" src="https://github.com/user-attachments/assets/f24a88ef-1e45-4ac6-b656-0377d8930e0a" />
<img width="986" height="337" alt="superpsic2" src="https://github.com/user-attachments/assets/00ec8437-ad53-427a-a188-c1fd654e4166" />

## 2. Keras

### Uso en el notebook

Keras es una biblioteca para construir, entrenar y evaluar redes neuronales mediante componentes como capas, optimizadores y funciones de pérdida. En el notebook se usa en **otro problema**: clasificar reseñas de películas del conjunto IMDB como negativas (0) o positivas (1). Por tanto, Keras es la herramienta utilizada, mientras que la red construida con ella es un modelo de **capas densas**, no una CNN de imágenes.
El código carga las reseñas con `imdb.load_data(num_words=10000)`: cada reseña llega como una secuencia de índices de palabras. La función `vectorizar` la convierte en un vector de 10 000 posiciones, con 1 si aparece una palabra y 0 si no aparece. Se reservan las primeras 10 000 reseñas del conjunto de entrenamiento para validación y se usan las restantes para entrenar. El conjunto de prueba queda para la evaluación posterior.

### Arquitectura, entrenamiento y evaluación

Con `models.Sequential()` se añaden dos capas `Dense(16, activation='relu')` y una capa final `Dense(1, activation='sigmoid')`. La salida `sigmoid` da un valor entre 0 y 1 para la clase positiva. El modelo se configura mediante `compile` con optimizador `rmsprop`, pérdida `binary_crossentropy` y métrica `accuracy`. En `fit` se entrenó durante **20 épocas**, con lotes de **512** y un conjunto de validación separado. Finalmente, `evaluate(x_test, y_test)` mide su desempeño en prueba.

El modelo principal obtuvo en prueba una **pérdida de 0,6007** y una **exactitud de 0,8583 (85,83 %)**. Al final del entrenamiento, la exactitud de entrenamiento mostrada en la época 20 fue **0,9987**, frente a **0,8701** en validación. La pérdida de entrenamiento siguió bajando, mientras que la de validación volvió a subir: es evidencia de **sobreajuste**, es decir, el modelo se adapta más a las reseñas usadas para entrenarlo que a reseñas nuevas. Conviene basar la cifra final de prueba en la salida de `model.evaluate`, ya que algunas anotaciones de texto del notebook la redondean de otra manera.

<img width="795" height="696" alt="curva crecimiento" src="https://github.com/user-attachments/assets/6bc3917a-f05f-4959-95e1-5090dd6bde17" />

El notebook también entrena una red más pequeña (una capa oculta de cuatro neuronas), otra con regularización L2 y otra con `Dropout(0.5)`. La gráfica comparativa de la red pequeña muestra una subida posterior menos pronunciada de la pérdida de validación que en el modelo original. Estas pruebas ilustran maneras de estudiar el sobreajuste; el notebook no presenta una evaluación en prueba para cada una, así que no corresponde afirmar que alguna superó al modelo original en prueba.

<img width="970" height="247" alt="código1111114" src="https://github.com/user-attachments/assets/fc228b75-1f0f-42aa-8669-90be391f8ff9" />
<img width="812" height="782" alt="222222222" src="https://github.com/user-attachments/assets/739d0079-f006-4a8a-825e-1feacf3bec9b" />

En `predictions[10]`, el modelo original devuelve **0,9856523** para una reseña de prueba. Es una puntuación alta para la clase positiva de **esa reseña concreta**; no es la exactitud global del modelo. La anotación posterior del notebook menciona 99,4 %, pero el valor que muestra la salida ejecutada equivale aproximadamente a 98,57 %.

## 3. Perceptrón

Un perceptrón es una unidad de decisión que recibe entradas numéricas, multiplica cada una por un *peso*, suma un **sesgo** y aplica una *función de activación* al resultado. En forma resumida: `salida = activación(entradas · pesos + sesgo)`. Los pesos determinan la influencia de cada entrada; el sesgo desplaza el umbral de decisión; y la activación transforma la suma en la salida deseada.
En el ejemplo del notebook, las entradas son **temperatura = 100** y **vibración = 50**, con pesos **[0,5; −0,5]** y sesgo **−30**. La suma ponderada es `100 × 0,5 + 50 × (−0,5) − 30 = −5`. Por eso la función escalón devuelve **0** (sin alerta), mientras que `tanh` devuelve aproximadamente **−0,9999** y también se interpreta como ausencia de alerta. **Es una demostración con valores definidos en el código**, no un modelo entrenado con observaciones reales.

Un perceptrón individual puede resolver decisiones binarias que se separan mediante un límite lineal. El notebook muestra ejemplos de compuertas AND y OR. También ilustra su limitación con XOR: un solo perceptrón no puede separar correctamente ese patrón; se necesita combinar varias neuronas y una capa de salida.
Su ventaja es la sencillez: requiere pocas operaciones y permite entender el efecto de entradas, pesos y umbral. Su limitación es que un perceptrón individual no aprende relaciones complejas ni patrones espaciales de una imagen como lo hace una CNN. Además, los pesos del ejemplo se escogieron manualmente; para afirmar que detecta alertas reales habría que definir y validar los criterios con datos.

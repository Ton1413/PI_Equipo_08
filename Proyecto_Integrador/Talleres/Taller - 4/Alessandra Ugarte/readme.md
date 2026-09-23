# Redes Neuronales

## Descripción

En este clase se estudio diferentes conceptos relacionados con las **redes neuronales y la inteligencia artificial**, utilizando ejemplos prácticos en Python. El objetivo fue comprender de manera sencilla cómo una red neuronal puede aprender a partir de datos y utilizar ese aprendizaje para realizar clasificaciones y predicciones.

Durante el desarrollo se trabajó principalmente con **redes neuronales convolucionales (CNN), clasificación de residuos, clasificación binaria, perceptrón, transfer learning y técnicas para evitar el sobreajuste**.

---

## Puntos importantes

### 1. Redes Neuronales Convolucionales (CNN)

Las CNN son redes neuronales especialmente útiles para trabajar con **imágenes**. Utilizan pequeños filtros llamados *kernels* que recorren la imagen y permiten encontrar características importantes, como bordes, formas y texturas.

En el proyecto se utilizaron las siguientes partes:

* **Conv2D:** encuentra características en las imágenes.
* **ReLU:** ayuda a que la red pueda aprender relaciones más complejas.
* **MaxPooling:** reduce el tamaño de la información manteniendo las características importantes.
* **Capas densas:** utilizan la información obtenida para realizar la clasificación.

Me pareció interesante porque una computadora puede aprender a reconocer características de una imagen sin que nosotros tengamos que indicarle manualmente dónde está cada característica.

---

### 2. Clasificación de residuos con TrashNet

Se utilizó el conjunto de datos **TrashNet** para trabajar en la clasificación de imágenes de residuos. En este caso, se consideraron dos categorías:

* **0 → Vidrio**
* **1 → Plástico**

Las imágenes fueron organizadas en diferentes grupos:

* **Entrenamiento:** imágenes utilizadas para que la red aprenda.
* **Validación:** imágenes utilizadas para observar cómo va funcionando el modelo durante el entrenamiento.
* **Prueba:** imágenes utilizadas al final para comprobar su desempeño con imágenes que no había utilizado para aprender.

El objetivo fue que la red neuronal pudiera **reconocer las características de una imagen y determinar si correspondía a vidrio o plástico**.

Esta parte me pareció especialmente interesante porque relaciona la **inteligencia artificial con un problema ambiental real**. La clasificación automática de residuos podría utilizarse como apoyo en sistemas de **separación, reciclaje y gestión de residuos**.

Además, me llamó la atención porque está relacionada directamente con mi carrera de **Ingeniería Ambiental**. Me permitió ver que lo aprendido sobre redes neuronales no solo sirve para trabajar con ejemplos de clase, sino que también puede aplicarse a problemas ambientales y buscar soluciones mediante la tecnología.

---

### 3. Evaluación del modelo

Para conocer cómo funciona el modelo se utilizaron diferentes métricas:

* **Accuracy:** porcentaje de predicciones correctas.
* **ROC-AUC:** permite observar qué tan bien el modelo diferencia las clases.
* **Matriz de confusión:** muestra los aciertos y errores.
* **Precision, Recall y F1:** permiten analizar con mayor detalle las predicciones.

En el entrenamiento de la CNN desde cero se observó una mejora gradual del modelo, alcanzando aproximadamente **63.27 % de exactitud** en el resultado indicado en el notebook.

Me pareció importante porque aprendí que no basta con entrenar una red neuronal y obtener una respuesta. También es necesario **evaluar qué tan bien está funcionando** y conocer dónde está cometiendo errores.

---

### 4. Data Augmentation

El *data augmentation* consiste en realizar pequeñas modificaciones a las imágenes, como rotaciones o desplazamientos, para generar variaciones de los datos.

Esto ayuda al modelo a **aprender mejor y generalizar** ante imágenes que no ha visto anteriormente.

Me pareció interesante porque permite aprovechar mejor un conjunto de imágenes sin tener que conseguir manualmente una gran cantidad de imágenes nuevas.

---

### 5. Transfer Learning

El **Transfer Learning** consiste en utilizar un modelo que ya fue entrenado anteriormente con una gran cantidad de imágenes y adaptarlo a un nuevo problema.

En el proyecto se trabajó con un modelo preentrenado y posteriormente se realizó *fine-tuning* para mejorar su aprendizaje.

Me pareció interesante porque permite utilizar conocimientos que un modelo ya aprendió y adaptarlos a un nuevo problema, sin tener que entrenar una red neuronal completamente desde cero.

---

### 6. Grad-CAM

Grad-CAM permite visualizar las zonas de una imagen que tuvieron mayor influencia en la predicción del modelo.

En el mapa de calor:

* **Zonas claras:** mayor importancia.
* **Zonas oscuras:** menor contribución.


Esta parte me pareció interesante porque permite entender **por qué la inteligencia artificial tomó una determinada decisión**, en lugar de simplemente observar el resultado final.

---

### 7. Clasificación de reseñas de películas

También se realizó una clasificación binaria utilizando **Keras**. En este caso, las reseñas de películas fueron clasificadas como:

* **Positivas**
* **Negativas**

Para trabajar con las palabras se utilizó **one-hot encoding** y una red neuronal con capas ocultas.

El modelo alcanzó aproximadamente **86.1 % de exactitud** en el resultado mostrado en el notebook.


Me pareció interesante porque demuestra que las redes neuronales no solamente pueden trabajar con imágenes, sino también con **texto y opiniones escritas por personas**.

---

### 8. Sobreajuste (Overfitting)

Uno de los problemas observados fue el **sobreajuste**.

Esto ocurre cuando el modelo aprende demasiado bien los datos de entrenamiento, pero tiene dificultades para trabajar con datos nuevos.

Para disminuir este problema se estudiaron técnicas como:

* Regularización.
* **Dropout**.
* Uso de modelos más pequeños.
* Aumento de datos.

En el *Dropout*, durante el entrenamiento se desactivan aleatoriamente algunas neuronas. Esto obliga a la red a buscar diferentes formas de aprender y puede ayudar a mejorar su generalización.



Me pareció importante porque aprendí que tener un buen resultado con los datos de entrenamiento no significa necesariamente que el modelo funcionará igual de bien con imágenes nuevas.

---

### 9. Perceptrón

El perceptrón es un modelo sencillo de inteligencia artificial que recibe entradas, las combina mediante **pesos** y genera una salida.

Se estudiaron diferentes funciones de activación y ejemplos relacionados con compuertas lógicas:

* **AND**
* **OR**
* **XOR**

Una observación importante fue que un solo perceptrón no puede resolver correctamente el problema **XOR**.

Sin embargo, utilizando más de un perceptrón y una capa adicional, sí es posible resolverlo.


Me pareció interesante porque permite entender desde un ejemplo sencillo cómo funcionan las decisiones de una neurona artificial y cómo, al combinar varias neuronas, se pueden resolver problemas más complejos.
---

## Lo que más me interesó del proyecto

Lo que más me llamó la atención fue observar que una red neuronal puede **aprender patrones a partir de los datos** y utilizarlos posteriormente para realizar predicciones.

En especial, me pareció interesante la **clasificación de residuos con TrashNet**, porque pude relacionar lo aprendido sobre inteligencia artificial con un problema relacionado directamente con la **Ingeniería Ambiental**.

También fue importante comprender que no basta con obtener una predicción. Es necesario evaluar el modelo, detectar problemas como el **sobreajuste** y buscar formas de mejorar su capacidad de generalización.




# Reconocimiento visual con SenseCraft AI y control de salida GPIO

**Evidencias para README I** · Implementación del modelo, pruebas de reconocimiento y configuración de GPIO.

## Integrantes del equipo

- **Jhosselyn Enriquez**
- **Alessandra Ugarte**
- **Pedro Cueva**

## Índice

1. [Objetivo](#objetivo)
2. [Implementación del modelo](#implementación-del-modelo)
3. [Pruebas y evidencias](#pruebas-y-evidencias)
4. [Interpretación de la confianza](#interpretación-de-la-confianza)
5. [Configuración de la salida GPIO](#configuración-de-la-salida-gpio)
6. [Resultados y verificación](#resultados-y-verificación)
7. [Conclusiones](#conclusiones)


## Objetivo

Implementar y probar un modelo de reconocimiento visual en **SenseCraft AI**, registrar evidencias de su funcionamiento con integrantes del equipo y configurar una salida GPIO para responder a un evento de reconocimiento.

## Implementación del modelo

Se utilizó el entorno de ejecución de **SenseCraft AI** para visualizar imágenes de la cámara, ejecutar inferencias y obtener las categorías y puntuaciones de confianza del modelo.

El proceso mostrado en las evidencias consiste en:

1. Ejecutar el modelo desde la interfaz de SenseCraft AI.
2. Mostrar la transmisión de la cámara en **Vista previa**.
3. Detectar los rostros y mostrar su etiqueta con una puntuación de confianza.
4. Consultar el **Registro del dispositivo** para observar los datos generados durante las inferencias.
5. Configurar las condiciones de activación de la salida GPIO.

**Nota técnica:** Las etiquetas `male` y `female` son las predicciones que muestra el modelo. No identifican personalmente a los participantes ni constituyen una verificación de su identidad o género.

## Pruebas y evidencias

### Evidencia 1. Reconocimiento individual

La primera captura presenta a una persona frente a la cámara. El modelo dibuja una caja de detección y muestra la etiqueta **`female: 74`**, es decir, una puntuación de confianza de **74** sobre una escala de 0 a 100. A la derecha se observan registros de detección y rendimiento del dispositivo.

<img width="1338" height="842" alt="image" src="https://github.com/user-attachments/assets/32a2b0b8-c1bb-4557-bef6-ff7402c5d7be" />

[Alessandra Ugarte]
### Evidencia 2. Reconocimiento simultáneo de dos personas

En la segunda prueba, el sistema muestra **dos detecciones en una misma imagen**: una con etiqueta **`male: 73`** y otra con etiqueta **`female: 79`**. Cada detección aparece delimitada por su propia caja. El registro del dispositivo muestra información correspondiente al procesamiento de las imágenes.

Esta evidencia permite comprobar que el modelo puede mostrar predicciones para **dos participantes simultáneamente**.
<img width="1347" height="831" alt="image" src="https://github.com/user-attachments/assets/da42fd88-daab-41c3-9382-699e8d1756bb" />
[Pedro Cueva]   [Jhosselyn Enriquez]

### Evidencia 3. Ajustes de activación GPIO

La tercera captura muestra el apartado **Transmitir datos a través de GPIO** y sus reglas de activación para las categorías `femenino` y `masculino`. Se incluyen las condiciones de confianza y los cambios de estado previstos para las salidas.

<img width="1302" height="470" alt="image" src="https://github.com/user-attachments/assets/ccb0d64b-6f2a-4ca8-a127-af1e1dcf28be" />


## Interpretación de la confianza

La **confianza** es la puntuación que el modelo asigna a cada detección. En estas capturas se presenta en una escala de **0 a 100**: un número mayor significa que el modelo asignó una puntuación más alta a esa predicción. **No equivale a una garantía de que la clasificación sea correcta.**

| Prueba | Etiqueta mostrada | Confianza observada |
|:--|:--|--:|
| Alessandra | `female` | **74** |
| Simultánea (Jhair) | `male` | **73** |
| Simultánea (Jhosselyn ) | `female` | **79** |

### ¿Cómo influye en la activación?

El **umbral de confianza** permite decidir cuándo una detección satisface una regla. Según la configuración fotografiada:

- Para la categoría **femenino**, se estableció la condición **confianza mayor que 41**. Por tanto, las detecciones `female: 74` y `female: 79` cumplen esa condición.
- Para la categoría **masculino**, se estableció la condición **confianza menor que 52**. Por tanto, la detección `male: 73` **no** cumple esa condición en la prueba fotografiada.

## Configuración de la salida GPIO

En **Acciones de activación** se observan los siguientes ajustes:

| Categoría | Condición de confianza | Estado predeterminado | Estado activo |
|:--|:--|:--|:--|
| Femenino | Mayor que **41** | LED apagado | **LED encendido** |
| Masculino | Menor que **52** | Nivel bajo | **Nivel alto** |

La segunda fila se muestra asociada a la salida **GPIO1(D0)**. En la primera, la interfaz muestra una referencia a **LED** (traducida como «CONDUJO» en la columna de GPIO de la captura).

**Funcionamiento esperado de la configuración femenina:** cuando el dispositivo evalúe una detección de categoría `femenino` con confianza superior a 41 y se cumplan las condiciones del sistema, la salida configurada deberá pasar de **LED apagado** a **LED encendido**.

> **Alcance de la evidencia:** La captura documenta el **ajuste GPIO**, pero ninguna de las tres imágenes muestra físicamente el **LED encendido**. Por ello, estas evidencias no bastan por sí solas para comprobar visualmente la activación física exigida en ese punto de la rúbrica.

## Resultados y verificación

| Criterio de la actividad | Evidencia incluida | Estado |
|:--|:--|:--|
| Implementar un modelo de SenseCraft AI | Capturas de la ejecución con detecciones y registros | **Documentado** |
| Realizar pruebas con 2 a 3 integrantes | Captura con dos personas detectadas simultáneamente | **Documentado** |
| Mostrar la configuración de la salida GPIO | Captura de condiciones, confianza y estados | **Documentado** |
| Mostrar físicamente el LED encendido durante el reconocimiento | No aparece en las tres imágenes seleccionadas | **Pendiente de evidencia** |

### Procedimiento de comprobación

Para verificar el sistema en una nueva demostración:

1. Abrir el modelo en **SenseCraft AI** y empezar la vista previa.
2. Situar una o dos personas delante de la cámara y observar las cajas, etiquetas y puntuaciones.
3. Revisar las reglas configuradas en **Transmitir datos a través de GPIO**.
4. Comprobar en el hardware que el LED cambia de estado cuando se cumple su regla y fotografiarlo encendido si la evaluación requiere esa evidencia.

## Conclusiones

Las capturas documentan la ejecución de un modelo de visión artificial en **SenseCraft AI**, tanto con una detección individual como con dos detecciones simultáneas. Los resultados muestran puntuaciones de confianza de **74, 73 y 79**, y se observa la configuración de condiciones de activación GPIO. Para completar toda la evidencia solicitada en la rúbrica, aún sería necesaria una fotografía donde se vea **el LED físicamente encendido** durante el reconocimiento.



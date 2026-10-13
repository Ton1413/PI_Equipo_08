# Clasificación de objetos con IA — PHONE y MOUSE

## Descripción del proyecto

Este proyecto documenta un modelo de **clasificación de imágenes** trabajado en **SenseCraft AI**, orientado a distinguir entre dos clases de objetos: **PHONE** (teléfono celular) y **MOUSE** (ratón de computadora). Se presentan capturas de la inferencia del modelo y se reserva espacio para compartir el conjunto de datos, el archivo del modelo y la evidencia de pruebas GPIO.

## Objetivos

- Preparar un conjunto de imágenes etiquetadas para las clases PHONE y MOUSE.
- Crear y probar un modelo de IA en SenseCraft AI.
- Mostrar el funcionamiento del reconocimiento en la vista de ejecución del dispositivo.
- Documentar las pruebas de salidas GPIO realizadas por los integrantes del grupo.
- Conservar el dataset, el modelo y sus evidencias en el repositorio de GitHub.

## Herramientas

| Herramienta / componente | Uso |
|---|---|
| SenseCraft AI | Plataforma para configurar y probar el modelo |
| Cámara / vista del dispositivo | Captura de imágenes para clasificación |
| GitHub | Almacenamiento y documentación del proyecto |

**Modelo exacto de dispositivo utilizado:** `XIAO ESP32S3 Sense`.

## 1. Creación del modelo de IA

Se presenta un clasificador de dos categorías:

| Clase | Objeto |
|---|---|
| `PHONE` | Teléfono celular |
| `MOUSE` | Ratón de computadora |

<img width="1281" height="906" alt="Captura de pantalla 2026-10-13 182912" src="https://github.com/user-attachments/assets/db3dbfc4-1aeb-4552-8a7f-b0e3b6da588d" />

**Proceso a documentar por el equipo:**

1. Reunir fotografías de ambos objetos.
2. Asignar cada imagen a su clase correspondiente.
3. Importar o capturar las imágenes en SenseCraft AI.
4. Configurar y entrenar el modelo.
5. Ejecutar pruebas con objetos reales.
6. Exportar o guardar los archivos disponibles del modelo.

## 2. Archivo ZIP de datos de entrenamiento

**Ubicación para subirlo:** [`dataset/dataset_phone_mouse.zip`](dataset/)

----

### Prueba de PHONE

<img width="1855" height="963" alt="Captura de pantalla 2026-10-13 180936" src="https://github.com/user-attachments/assets/10666eca-239d-40bf-b926-0892099416db" />

**Resultado observado en la captura:** `PHONE = 100%`, `MOUSE = 0%`.

### Prueba de MOUSE

<img width="1852" height="962" alt="Captura de pantalla 2026-10-13 180841" src="https://github.com/user-attachments/assets/cecec6e1-7bc4-492c-b881-97f45e1c40bd" />

**Resultado observado en la captura:** `MOUSE = 100%`, `PHONE = 0%`.

| Evidencia visible | Resultado |
|---|---|
| Clases | PHONE y MOUSE |
| Confianza mostrada — teléfono | 100% PHONE |
| Confianza mostrada — mouse | 100% MOUSE |
| Umbral configurado en las capturas | 24 (escala 0–100) |

**Interpretación:** Las capturas muestran clasificaciones correctas de los dos objetos en esos dos ejemplos. El 100% indicado corresponde a la **confianza mostrada en cada inferencia**, no a una exactitud general del modelo medida sobre un conjunto de pruebas.

## Resultados y conclusiones

Las dos pruebas compartidas muestran que el modelo asignó la clase `PHONE` al teléfono y `MOUSE` al ratón, con una confianza visualizada del **100% en cada ejemplo**. También se observan registros continuos de inferencia en la interfaz.

Estos resultados evidencian el funcionamiento de la clasificación en las situaciones mostradas; **no permiten afirmar una exactitud global del 100%**. Para completar la entrega deben añadirse el ZIP de imágenes, los archivos o enlace del modelo, y la documentación real de las pruebas GPIO.


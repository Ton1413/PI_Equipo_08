
# TRL 3: Desarrollo experimental y prueba de concepto de RECONEXA

## 1. Descripción de las pruebas

El equipo RECONEXA realizó pruebas individuales de componentes electrónicos utilizando Arduino UNO y Arduino IDE. Se evaluaron funciones de detección magnética, medición de distancia y visualización de información, además del montaje inicial del sistema de pesaje.

## 2. Evidencias experimentales

### 2.1. Prueba del sensor de efecto Hall

Se conectó el sensor Hall al Arduino UNO para comprobar su respuesta ante un campo magnético. En el monitor serial se registraron valores digitales de **0 y 1**, que muestran cambios en la señal del sensor.

![Conexión del sensor Hall](imagenes/sensor_hall.jpg)

**Figura 1.** Conexión del sensor Hall al Arduino UNO.

![Resultados del sensor Hall](imagenes/resultados_hall.jpg)

**Figura 2.** Registro de valores digitales 0 y 1 en Arduino IDE.

### 2.2. Prueba del sensor de distancia VL53L0X

Se utilizó el sensor VL53L0X para medir distancias. En el monitor serial se observaron valores entre **84 y 117 mm**, equivalentes a **8,4 y 11,7 cm**.

![Prueba del sensor VL53L0X](imagenes/sensor_distancia.jpg)

**Figura 3.** Prueba del sensor de distancia mediante Arduino UNO.

![Resultados de distancia](imagenes/resultados_distancia.jpg)

**Figura 4.** Lecturas de distancia registradas en Arduino IDE.

### 2.3. Montaje de la celda de carga

Se realizó la conexión inicial de la celda de carga con el módulo electrónico y el Arduino UNO. La fotografía muestra el montaje físico utilizado para preparar las pruebas de pesaje.

![Montaje de celda de carga](imagenes/celda_carga.jpg)

**Figura 5.** Montaje inicial de la celda de carga.

### 2.4. Prueba de pantalla TFT

Se programó una pantalla TFT de **128 × 160 píxeles** mediante Arduino IDE. Se realizaron pruebas de visualización de colores y texto, mostrando el mensaje **"HOLA!"** junto con la identificación del Arduino UNO.

![Código de pantalla TFT](imagenes/codigo_tft.jpg)

**Figura 6.** Programación de la pantalla TFT en Arduino IDE.

![Resultado de pantalla TFT](imagenes/pantalla_tft.jpg)

**Figura 7.** Visualización del mensaje de prueba en pantalla TFT.

*Fuente de las figuras: elaboración del equipo RECONEXA.*

## 3. Resultados del TRL 3

Las pruebas individuales permitieron registrar cambios digitales del sensor Hall, obtener mediciones de distancia con el VL53L0X y visualizar información en la pantalla TFT.

También se documentó el montaje inicial de la celda de carga. Estas evidencias corresponden a las primeras pruebas experimentales de RECONEXA y respaldan su desarrollo en el TRL 3.

La calibración del sistema de pesaje y el funcionamiento integrado de los componentes aún no se encuentran demostrados.

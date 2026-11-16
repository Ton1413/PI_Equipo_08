
# TRL 3: Desarrollo experimental y prueba de concepto de RECONEXA

## 1. Descripción de las pruebas

El equipo RECONEXA realizó pruebas individuales de componentes electrónicos utilizando Arduino UNO y Arduino IDE. Se evaluaron funciones de detección magnética, medición de distancia y visualización de información, además del montaje inicial del sistema de pesaje.

## 2. Evidencias experimentales

### 2.1. Prueba del sensor de efecto Hall

Se conectó el sensor Hall al Arduino UNO para comprobar su respuesta ante un campo magnético. En el monitor serial se registraron valores digitales de **0 y 1**, que muestran cambios en la señal del sensor.

<img width="1152" height="1536" alt="image" src="https://github.com/user-attachments/assets/f541a12a-d1ad-4e6f-a2d2-252a447dcea6" />
**Figura 1.** Conexión del sensor Hall al Arduino UNO.


<img width="1536" height="1152" alt="image" src="https://github.com/user-attachments/assets/25f2966d-0562-4e3d-ba55-ccec7b720958" />


**Figura 2.** Registro de valores digitales 0 y 1 en Arduino IDE.
### 2.2. Prueba del sensor de distancia VL53L0X

Se utilizó el sensor VL53L0X para medir distancias. En el monitor serial se observaron valores entre **84 y 112 mm**, equivalentes a **8,4 y 11,20 cm**.

<img width="1536" height="1152" alt="image" src="https://github.com/user-attachments/assets/63fe80fa-89c2-4576-8ac6-8b53fa2e9d9f" />


**Figura 3.** Prueba del sensor de distancia mediante Arduino UNO.

<img width="1536" height="864" alt="imagen" src="https://github.com/user-attachments/assets/bc563ec3-12d3-4e19-80ab-62db629167f3" />


**Figura 4.** Lecturas de distancia registradas en Arduino IDE.

### 2.3. Montaje de la celda de carga

Se realizó la conexión inicial de la celda de carga con el módulo electrónico y el Arduino UNO. La fotografía muestra el montaje físico utilizado para preparar las pruebas de pesaje.

<img width="1152" height="1536" alt="image" src="https://github.com/user-attachments/assets/a48913d8-a2a9-41a2-ac9e-f0c17a4a8f4d" />


**Figura 5.** Montaje inicial de la celda de carga.

### 2.4. Prueba de pantalla TFT

Se programó una pantalla TFT de **128 × 160 píxeles** mediante Arduino IDE. Se realizaron pruebas de visualización de colores y texto, mostrando el mensaje **"HOLA!"** junto con la identificación del Arduino UNO.

<img width="1152" height="1536" alt="image" src="https://github.com/user-attachments/assets/6cc28ec1-84c1-4dd8-9707-a65f0387b5b4" />


**Figura 6.** Programación de la pantalla TFT en Arduino IDE.

<img width="1152" height="1536" alt="image" src="https://github.com/user-attachments/assets/67e54a82-1501-45a1-a019-5bd8ae2a54f3" />

**Figura 7.** Visualización del mensaje de prueba en pantalla TFT.

*Fuente de las figuras: elaboración del equipo RECONEXA.*

## 3. Resultados del TRL 3

Las pruebas individuales permitieron registrar cambios digitales del sensor Hall, obtener mediciones de distancia con el VL53L0X y visualizar información en la pantalla TFT.

También se documentó el montaje inicial de la celda de carga. Estas evidencias corresponden a las primeras pruebas experimentales de RECONEXA y respaldan su desarrollo en el TRL 3.

La calibración del sistema de pesaje y el funcionamiento integrado de los componentes aún no se encuentran demostrados.

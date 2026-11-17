
# TRL 4: Integración y validación de componentes en laboratorio

## 1. Desarrollo experimental

Se realizó el montaje del subsistema de pesaje de RECONEXA, integrando una celda de carga, el módulo HX711 y Arduino UNO.

El montaje permitió registrar mediciones de peso mediante Arduino IDE y evaluar su funcionamiento utilizando masas de referencia conocidas.

## 2. Evidencias experimentales

### 2.1. Armado del subsistema de pesaje

<img width="1195" height="1024" alt="capture_temp" src="https://github.com/user-attachments/assets/b426304e-5b62-4c38-8738-f7c2c00e022d" />


**Figura 1. Armado experimental de la celda de carga.**

La imagen muestra la estructura de soporte utilizada para colocar la celda de carga y realizar las pruebas de pesaje. Se emplearon masas conocidas para comprobar la respuesta del sensor ante diferentes cargas. Este montaje permitió mantener una configuración de prueba para evaluar el sistema en laboratorio.

### 2.2. Conexión de los componentes electrónicos

<img width="1121" height="1001" alt="capture_temp" src="https://github.com/user-attachments/assets/b0fa1680-2529-466a-8ee6-e84212c9d680" />

**Figura 2. Conexión de Arduino UNO, HX711 y celda de carga.**

Se observa la conexión entre los componentes del subsistema de pesaje. El módulo HX711 recibe la señal de la celda de carga y la convierte en datos digitales que pueden ser procesados por Arduino UNO. Esta integración permitió obtener lecturas de peso mediante un programa de prueba.

### 2.3. Calibración y registro de resultados

<img width="1173" height="1051" alt="capture_temp" src="https://github.com/user-attachments/assets/1034a0fc-d09a-4df5-b5b7-d93bc78b0c2d" />


**Figura 3. Resultados de calibración registrados en Arduino IDE.**

Se realizaron pruebas con pesas de 0, 5, 10, 20 y 50 g. Los resultados se observaron en Arduino IDE, donde se registraron las mediciones y los errores obtenidos.

### 2.4. Evaluación del armado completo

<img width="1315" height="1057" alt="capture_temp" src="https://github.com/user-attachments/assets/30780b16-452c-4259-a8e6-41f144298d01" />


**Figura 4. Vista general del subsistema de pesaje durante las pruebas.**

La fotografía presenta el montaje experimental con la celda de carga, el módulo HX711, Arduino UNO y las masas de referencia. La disposición de estos componentes permitió realizar pruebas de funcionamiento y registrar las mediciones obtenidas en laboratorio.

## 3. Resultados del avance tecnológico

Las pruebas demostraron el funcionamiento conjunto de la celda de carga, el módulo HX711 y Arduino UNO, obteniéndose mediciones de peso con masas de referencia conocidas.

Estos resultados constituyen un avance en la integración de componentes básicos de RECONEXA. La validación del TRL 4 se completará cuando se demuestre el funcionamiento conjunto de los demás sensores del prototipo.




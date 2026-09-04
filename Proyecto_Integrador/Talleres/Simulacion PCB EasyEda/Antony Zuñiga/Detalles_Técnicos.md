# Diseño electrónico

## 1. Importancia del circuito

Este circuito permite detectar muestras metálicas y realizar el acondicionamiento seguro de señales para el sistema IoT. Debido a que la etapa de entrada opera con una fuente externa recibida por el conector Jack DC1, es indispensable aislar eléctricamente esta señal a través del optoacoplador U2 para proteger los pines de entrada del microcontrolador U1 (ESP32-DevKitC) y evitar interferencias o sobrevoltajes.

## 2. Componentes utilizados

| Componente | Designador | Función |
|---|---|---|
| ESP32-DevKitC | **U1** | Procesador principal del sistema IoT |
| Optoacoplador PC817 | **U2** | Aísla ópticamente la señal de entrada respecto al ESP32 |
| Jack DC-005 | **DC1** | Entrada de alimentación externa |
| Borniera de 2 pines | **H1** | Conexión para la señal de entrada del sensor |
| Resistencia de 1 kΩ | **R1** | Limita la corriente hacia el LED interno del optoacoplador U2 |
| Resistencia de 10 kΩ | **R2** | Mantiene la línea en *pull-up* a 3.3V en el pin de datos |
| Orificios de montaje | **MH1, MH2, MH3, MH4** | Agujeros de fijación mecánica M3 para la PCB |

## 3. Diseño esquemático

El esquemático fue diseñado en EasyEDA integrando las conexiones entre el conector de alimentación DC1, la borniera H1, el optoacoplador U2 y el módulo U1.

### Conexiones del esquemático

| Origen | Destino | Función |
|---|---|---|
| **DC1 (Pin 1)** | Borniera H1 (Pin 1) | Alimentación positiva de la etapa de entrada |
| **DC1 (Pin 2)** | U2 (Pin 2) y U1 GND (Pin 14) | Tierra de referencia compartida del circuito |
| **Borniera H1 (Pin 2)** | Resistencia R1 (1 kΩ) | Paso de la señal de entrada hacia la resistencia limitadora |
| **Resistencia R1** | U2 Pin 1 (Ánodo) | Alimentación del LED del optoacoplador PC817 |
| **U2 Pin 3 (Emisor)** | U1 GND (Pin 26) | Conexión del emisor a la tierra del ESP32 |
| **U2 Pin 4 (Colector)** | U1 IO18 (Pin 29) | Lectura digital del estado del sensor |
| **U1 3V3 (Pin 1)** | Resistencia R2 (10 kΩ) | Alimentación del circuito *pull-up* |
| **Resistencia R2** | U2 Pin 4 / U1 IO18 | Mantiene en nivel alto (3.3V) el pin IO18 en reposo |

Se colocaron banderas de no conexión (`X`) en todos los pines no utilizados de U1, DC1 y en los orificios MH1-MH4 para asegurar la integridad en la prueba DRC.

<img width="767" height="507" alt="FIG1" src="https://github.com/user-attachments/assets/53025d64-9b25-4a74-ad09-7ebfaab6ede1" />


---

## 4. Conversión del esquemático a PCB

Tras verificar las conexiones y superar la comprobación DRC en EasyEDA, el esquemático fue convertido al entorno de diseño de PCB.

Los componentes se organizaron siguiendo estos criterios:

* **DC1 y H1:** Posicionados cerca de los bordes para facilitar la conexión de la fuente de poder y el sensor.
* **U2, R1 y R2:** Agrupados cerca del puerto de entrada para mantener rutas de señal cortas hacia el microcontrolador.
* **U1 (ESP32):** Ubicado de forma centralizada.
* **Agujeros MH1 a MH4:** Distribuidos en las esquinas para permitir la sujeción firme de la placa con tornillos M3.

<img width="535" height="371" alt="FIG2" src="https://github.com/user-attachments/assets/c0377968-aa33-4000-9d03-ff3841716146" />


---

## 5. Modelo 3D

La vista 3D permite validar la disposición espacial del circuito antes de mandar a fabricar la tarjeta.

Se verifica lo siguiente:

* Los componentes no presentan superposiciones físicas.
* El conector DC1 y la borniera H1 quedan accesibles para el cableado externo.
* El puerto USB del ESP32 (U1) permanece despejado.
* Los cuatro agujeros M3 (MH1-MH4) están libres para el montaje en chasis o caja protectora.

<img width="1282" height="621" alt="FIG3" src="https://github.com/user-attachments/assets/b840e0be-5474-401f-93d2-7f0f8bdbf09b" />


---

## 6. Funcionamiento dentro del proyecto

Cuando se activa la señal en la borniera H1, la corriente fluye a través de la resistencia R1 (1 kΩ) encendiendo el LED interno del optoacoplador U2. 

Esto activa el fototransistor interno de U2, derivando a tierra (`GND`) el voltaje del pin `IO18` del ESP32 (U1). El microcontrolador detecta este cambio de estado (de 3.3V a 0V) y procesa la lectura dentro de la lógica del sistema.

---

## 7. Conclusión

El diseño esquemático establece un circuito de entrada aislado por optoacoplador, la PCB organiza los componentes U1, U2, DC1, H1, R1, R2 y los orificios MH1-MH4 de forma práctica, y la validación 3D garantiza un ensamblaje físico seguro.

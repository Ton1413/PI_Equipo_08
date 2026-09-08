| MATRIZ MORFOLÓGICA                                                                   |
|--------------------|-----------------------------------|-------------------------------------|-----------------------------------------|--------------------------------------------------|
|                    |                                   |                                     |                                         |                                                  |
| N.º                | Subfunción                        | Alternativa 1                       | Alternativa 2                           | Alternativa 3                                    |
| 1                  | Recibir y posicionar la muestra   | Bandeja fija no metálica            | Soporte ajustable con abrazadera        | Recipiente removible normalizado                 |
| 2                  | Iniciar el ciclo de medición      | Pulsador físico                     | Sensor de presencia                     | Comando desde interfaz web                       |
| 3                  | Medir el peso de la muestra       | Celda de carga 5 kg y HX711         | Celda de carga 1 kg y NAU7802           | Celda industrial y transmisor 4–20 mA            |
| 4                  | Capturar la imagen de la muestra  | ESP32-CAM con OV2640                | ArduCam Mini 2 MP con OV2640            | Raspberry Pi Camera Module 3                     |
| 5                  | Estimar el volumen de la muestra  | HC-SR04                             | Sensor ToF VL53L0X                      | JSN-SR04T en depósito de desplazamiento          |
| 6                  | Medir la respuesta inductiva      | Oscilador LC y ADC del ESP32        | LDC1612 y  bobina de sensado            | LDC1000 y bobina de sensado                      |
| 7                  | Compensar condiciones ambientales | DHT22 (temperatura y humedad)       | BME280 (temperatura, humedad y presión) | SHT31 (temperatura y humedad de mayor precisión) |
| 8                  | Acondicionar las señales          | Módulos comerciales de sensor       | Circuito analógico diseñado en la PCB   | Sensores digitales calibrados                    |
| 9                  | Procesar la información           | ESP32                               | Raspberry Pi                            | ESP32 con procesamiento en la nube               |
| 10                 | Clasificar la muestra             | Comparación con rangos establecidos | Árbol de decisión                       | Modelo de aprendizaje automático                 |
| 11                 | Comunicar la información          | Wi-Fi                               | Ethernet                                | LoRa mediante gateway                            |
| 12                 | Transferir los datos              | MQTT                                | HTTP/REST                               | Almacenamiento local y sincronización periódica  |
| 13                 | Almacenar las mediciones          | Memoria interna del ESP32           | Tarjeta microSD                         | Nube con respaldo local                          |
| 14                 | Mostrar el resultado localmente   | LED RGB                             | LED RGB y buzzer                        | Pantalla OLED o LCD                              |
| 15                 | Mostrar el resultado remotamente  | Dashboard web                       | Aplicación móvil                        | BotAI                                            |
| 16                 | Alimentar el sistema              | Adaptador AC-DC de 12 V             | Fuente USB-C de 5 V                     | Panel solar con batería                          |
| 17                 | Regular el voltaje                | Convertidor buck LM2596             | Convertidor buck MP1584                 | Regulador LDO de 3.3 V                           |
| 18                 | Proteger físicamente el sistema   | Carcasa para interiores             | Gabinete metálico industrial            | Caja estanca IP65                                |

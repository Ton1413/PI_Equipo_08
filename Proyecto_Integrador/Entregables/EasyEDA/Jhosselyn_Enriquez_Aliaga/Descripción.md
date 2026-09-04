# Diseño electrónico del sistema de monitoreo

## 1. Importancia del circuito

Este circuito permite medir la temperatura y humedad del entorno durante el funcionamiento del proyecto. Estas variables pueden influir en la conductividad, corrosión y estabilidad de las mediciones realizadas sobre materiales como cobre, latón y aluminio.

> **Nota:** El DHT22 no detecta metales directamente. Su función es registrar las condiciones ambientales y complementar las mediciones de los sensores principales.

## 2. Componentes utilizados

| Componente | Función |
|---|---|
| ESP32 DevKit V1 | Recibe y procesa los datos |
| DHT22 | Mide temperatura y humedad |
| Resistencia de 10 kΩ | Estabiliza la línea de datos |
| PCB | Integra las conexiones del circuito |
| Cuatro agujeros M3 | Permiten sujetar la PCB |

## 3. Diseño esquemático

El esquemático fue elaborado en EasyEDA para representar las conexiones eléctricas entre el ESP32 y el DHT22.

### Conexiones

| Pin del DHT22 | Conexión |
|---|---|
| VCC | 3.3 V del ESP32 |
| DATA | GPIO27 del ESP32 |
| NC | Sin conexión |
| GND | GND del ESP32 |

También se colocó una resistencia de **10 kΩ** entre `VCC` y `DATA` para mantener estable la comunicación entre el sensor y el ESP32.

<p align="center">
 <img width="742" height="522" alt="Figura 1  Esquemático del ESP32 conectado al sensor DHT22" src="https://github.com/user-attachments/assets/c651ccc6-64f9-4f8d-966d-d849eabfafc8" />
  <br>
  <em><b>Figura 1.</b> Esquemático del ESP32 conectado al sensor DHT22.</em>
</p>

## 4. Conversión del esquemático a PCB

Después de verificar las conexiones, el esquemático fue convertido a PCB en EasyEDA.

Los componentes fueron distribuidos dejando espacio suficiente para evitar superposiciones. Las pistas se configuraron con un ancho de **20 mil** y se añadieron cuatro agujeros M3 para sujetar la tarjeta dentro de su estructura.

El DHT22 se ubicó separado del ESP32 para evitar que el calor generado por el microcontrolador altere las mediciones.

<p align="center">
 <img width="492" height="482" alt="Figura 2  Distribución de componentes y pistas en la PCB" src="https://github.com/user-attachments/assets/362a300a-c9ec-49f5-9b42-6ed309312f31" />
  <br>
  <em><b>Figura 2.</b> Distribución de componentes y pistas en la PCB.</em>
</p>

## 5. Modelo 3D

La vista 3D permitió comprobar la disposición física de los componentes antes de fabricar la PCB.

Se verificó lo siguiente:

- Los componentes no se superponen.
- El puerto USB del ESP32 permanece accesible.
- Existe espacio para realizar las soldaduras.
- Los agujeros M3 están ubicados en las esquinas.
- La placa puede instalarse dentro de una caja de protección.

<p align="center">
<img width="1252" height="726" alt="Figura 3  Modelo 3D del circuito diseñado en EasyEDA" src="https://github.com/user-attachments/assets/0da286fe-918f-48ba-9b10-d9ccf0898977" />
  <br>
  <em><b>Figura 3.</b> Modelo 3D del circuito diseñado en EasyEDA.</em>
</p>

## 6. Funcionamiento dentro del proyecto

El DHT22 mide la temperatura y humedad del entorno y envía estos datos al ESP32 mediante el pin `GPIO27`.

El ESP32 puede procesar esta información junto con las mediciones obtenidas por los sensores principales. Esto permite determinar si las condiciones ambientales están afectando el análisis de materiales como cobre, latón y aluminio.

> **Aclaración técnica:** El aluminio no se clasifica normalmente como un metal pesado y el latón es una aleación formada principalmente por cobre y zinc. Para identificar estos materiales se necesitan sensores adicionales; el DHT22 solamente proporciona información ambiental complementaria.

## 7. Conclusión

El esquemático establece las conexiones eléctricas, la PCB organiza físicamente el circuito y el modelo 3D permite comprobar el montaje antes de su fabricación.

En conjunto, este diseño proporciona un módulo de monitoreo ambiental que complementa el sistema utilizado para analizar diferentes materiales metálicos.

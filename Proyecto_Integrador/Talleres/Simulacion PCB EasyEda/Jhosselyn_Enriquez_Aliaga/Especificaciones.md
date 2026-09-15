# Diseño electrónico del módulo de monitoreo ambiental

## 1. Importancia del circuito

Este circuito permite medir la temperatura y la humedad del entorno durante el funcionamiento de **RECONEXA**, sistema orientado a la caracterización preliminar de muestras metálicas de cobre, aluminio y latón.

Estas condiciones ambientales pueden influir en la estabilidad de algunos sensores y componentes electrónicos. Además, la humedad puede favorecer procesos de corrosión superficial. Por ello, el DHT22 proporciona información complementaria sobre las condiciones en las que se realiza cada evaluación.

## 2. Identificación del diseño

| Campo | Información |
|---|---|
| **Universidad** | Universidad Peruana Cayetano Heredia |
| **Proyecto** | RECONEXA |
| **Equipo** | Equipo N.º 08 |
| **Módulo** | Monitoreo ambiental con ESP32 y DHT22 |
| **Alumna** | Jhosselyn Dayanna Enriquez Aliaga |
| **Docente** | Mg. Umbert Lewis de la Cruz Rodríguez |
| **Jefe de práctica** | Ing. Renzo José Chan Ríos |
| **Jefa de práctica** | Ing. María Angélica Rejas Núñez |
| **Coordinador adjunto** | Dr. Harry Anderson Rivera Tito |
| **Versión** | V1.1 |
| **Fecha de actualización** | 15/09/2026 |

---

## 3. Componentes utilizados

| Componente | Función |
|---|---|
| **ESP32 DevKit V1** | Recibe, procesa y permite transmitir los datos ambientales |
| **DHT22** | Mide la temperatura y la humedad relativa del entorno |
| **Resistencia de 10 kΩ** | Funciona como resistencia *pull-up* de la línea de datos |
| **PCB** | Integra y organiza las conexiones físicas del circuito |
| **Cuatro agujeros M3** | Permiten fijar la PCB dentro de la estructura |
| **Pistas de cobre** | Conducen la alimentación y las señales eléctricas |

---

## 4. Diseño esquemático

El esquemático fue elaborado en **EasyEDA** para representar las conexiones eléctricas entre el ESP32 DevKit V1 y el sensor DHT22.

También se incorporó un cajetín con la identificación del proyecto, número de equipo, alumna, docentes, versión y fecha de actualización.

### 4.1 Conexiones eléctricas

| Pin del DHT22 | Conexión |
|---|---|
| **Pin 1 – VCC** | Pin 30 – 3.3 V del ESP32 |
| **Pin 2 – DATA** | GPIO27 del ESP32 |
| **Pin 3 – NC** | Sin conexión intencional |
| **Pin 4 – GND** | Pin 29 – GND del ESP32 |

Se colocó una resistencia de **10 kΩ entre VCC y DATA** para mantener la línea de datos en un nivel lógico estable cuando no se transmite información.

### 4.2 Pines sin conexión

El pin 3 del DHT22 está identificado como **NC (*Not Connected*)**, debido a que el fabricante establece que no debe conectarse eléctricamente.

También se colocaron indicadores de no conexión en los pines del ESP32 que no son utilizados por este módulo. Esto permite diferenciar los pines que se dejaron libres intencionalmente de posibles conexiones omitidas por error.

### 4.3 Verificación del esquemático

El diseño fue revisado mediante la herramienta **ERC (*Electrical Rule Check*)** de EasyEDA para detectar posibles conexiones incompletas, conflictos eléctricos o pines sin identificar.

<p align="center">
<img width="2362" height="1672" alt="SCH_Schematic1_1-P1_2026-09-15" src="https://github.com/user-attachments/assets/54b6f0f2-a8e8-451b-bf66-4ffd582e577b" />

  <br>
  <em><b>Figura 1.</b> Esquemático corregido del módulo ESP32–DHT22, incluyendo la identificación académica y los pines NC.</em>
</p>

---

## 5. Conversión del esquemático a PCB

Después de verificar las conexiones eléctricas, el esquemático fue convertido a una PCB en EasyEDA.

Los componentes fueron distribuidos dejando espacio suficiente para evitar superposiciones y facilitar el montaje. Las pistas se configuraron con un ancho de **20 mil** y se añadieron cuatro agujeros M3 para fijar la tarjeta dentro de su estructura.

El DHT22 se ubicó separado del ESP32 para reducir la posibilidad de que el calor generado por el microcontrolador altere la medición de la temperatura ambiental.

### 5.1 Mejoras incorporadas

La versión corregida de la PCB incluye:

- Cuatro agujeros de montaje M3.
- Cuatro esquinas redondeadas.
- Identificación en la capa **Top Silkscreen**.
- Nombre de la universidad y del proyecto.
- Número del equipo.
- Nombre abreviado de la alumna.
- Número de versión de la placa.
- Separación entre componentes, pads y borde exterior.
- Acceso libre al puerto USB del ESP32.

Las esquinas fueron redondeadas para eliminar vértices pronunciados, mejorar la manipulación de la tarjeta y facilitar su instalación dentro de la caja de protección.

### 5.2 Identificación en la PCB

En la capa superior de serigrafía se colocó la siguiente información:

```text
UPCH - RECONEXA
EQUIPO 08
J. D. ENRIQUEZ
```

### 5.3 Verificación de la PCB

Se ejecutó la herramienta **DRC (*Design Rule Check*)** para comprobar las separaciones entre pistas, pads, perforaciones y borde de la placa.

<p align="center">
<img width="1646" height="2589" alt="PCB_PCB1_2026-09-15" src="https://github.com/user-attachments/assets/80477d29-8699-43b2-ad80-3003665650e3" />
  <br>
  <em><b>Figura 2.</b> Distribución corregida de componentes, pistas, agujeros M3, identificación y esquinas redondeadas.</em>
</p>

---

## 6. Modelo 3D

La vista 3D permitió revisar la disposición física del circuito antes de su posible fabricación.

Se verificaron los siguientes aspectos:

- Los componentes no presentan superposiciones.
- El puerto USB del ESP32 permanece accesible.
- Existe espacio para realizar las soldaduras.
- Los cuatro agujeros M3 se encuentran próximos a las esquinas.
- Las esquinas de la PCB están redondeadas.
- La identificación es visible en la serigrafía superior.
- La placa puede instalarse dentro de una caja de protección.

<p align="center">
<img width="2160" height="2800" alt="3D_PCB1_2026-09-15" src="https://github.com/user-attachments/assets/1176e9ea-d40c-412f-ba06-829c684a82ea" />
  <br>
  <em><b>Figura 3.</b> Modelo 3D corregido del módulo de monitoreo ambiental.</em>
</p>

---

## 7. Funcionamiento dentro del proyecto

El DHT22 mide la temperatura y la humedad relativa del entorno. Posteriormente, transmite estas lecturas al ESP32 mediante el pin `GPIO27`.

El ESP32 procesa y registra esta información junto con los datos obtenidos por los sensores principales del sistema. De esta manera, las condiciones ambientales pueden asociarse con cada evaluación realizada sobre las muestras de cobre, aluminio y latón.

Esta información no sustituye la medición de las propiedades del metal ni realiza directamente su clasificación. Su propósito es aportar contexto ambiental y ayudar a detectar condiciones que podrían afectar la repetibilidad o estabilidad del proceso.

---

## 8. Conclusión

El esquemático representa las conexiones eléctricas del módulo, la PCB organiza físicamente sus componentes y el modelo 3D permite revisar el montaje antes de su fabricación.

Las correcciones realizadas incorporan la identificación académica del diseño, los pines NC, las esquinas redondeadas, la serigrafía de la PCB y las comprobaciones ERC y DRC.

En conjunto, el circuito funciona como un módulo complementario de monitoreo ambiental para **RECONEXA**, aportando información sobre las condiciones en las que se realiza la caracterización preliminar de muestras de cobre, aluminio y latón.

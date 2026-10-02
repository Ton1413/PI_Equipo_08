<div align="center">

<h1>Taller de Internet de las Cosas (IoT)</h1>
<p><strong>ESP32, sensores, WiFi y plataformas IoT</strong></p>

</div>

---

## Objetivo del taller

Trabajar con un **ESP32** para realizar adquisición de datos, conexión WiFi, envío de información a plataformas IoT y control de un actuador desde una interfaz web. La guía plantea cinco actividades principales.

---

## Materiales y herramientas

- ESP32 Dev Kit 1.
- Protoboard.
- Potenciómetro.
- Sensor LDR del kit Keystudio.
- LED y resistencia.
- Cables de conexión.
- Cable USB.
- Arduino IDE.
- Conexión WiFi.
- Cuenta/canal en las plataformas IoT utilizadas.
- Librerías `WiFi.h`, `ThingSpeak.h` y `WebServer.h`, según la actividad.


## Actividad 01 — Lectura del potenciómetro, promedio y voltaje

<details>
<summary><strong>Ver enunciado</strong></summary>

La actividad solicita mejorar el código básico de lectura del potenciómetro mediante un **promediado de los datos** y convertir los valores obtenidos por el ADC a **valores de voltaje**.

</details>

### Conexión

| Terminal del potenciómetro | ESP32 |
|---|---|
| Extremo | 3.3 V |
| Otro extremo | GND |
| Terminal central | GPIO 34 |

<details>
<summary><strong>Ver código</strong></summary>

```cpp
const int potPin = 34;        // Pin del potenciómetro
const int muestras = 10;      // Número de lecturas
const float Vref = 3.3;       // Referencia de voltaje
const int ADCmax = 4095;      // ADC de 12 bits

void setup() {
  Serial.begin(115200);
}

void loop() {

  long suma = 0;

  // Tomar 10 muestras
  for (int i = 0; i < muestras; i++) {
    suma += analogRead(potPin);
    delay(10);
  }

  // Calcular el promedio
  float promedio = suma / (float)muestras;

  // Convertir ADC a voltaje
  float voltaje = promedio * Vref / ADCmax;

  // Mostrar resultados
  Serial.print("ADC promedio: ");
  Serial.print(promedio);

  Serial.print(" | Voltaje: ");
  Serial.print(voltaje, 3);
  Serial.println(" V");

  delay(500);
}
```

</details>

<details>
<summary><strong>Explicación</strong></summary>

El ESP32 realiza diez lecturas consecutivas del potenciómetro mediante `analogRead()`. Estas lecturas se acumulan en `suma` y posteriormente se dividen entre el número de muestras para obtener el promedio.

La conversión utilizada es:

\[
V = \frac{ADC \times 3.3}{4095}
\]

donde `ADC` corresponde al valor promedio obtenido, `3.3` V es la referencia utilizada en el ejercicio y `4095` corresponde al máximo considerado para una lectura ADC de 12 bits.

El promedio permite obtener una lectura más estable que una única medición.

</details>
<details>
<summary><strong>Resultado</strong></summary>

En el monitor serial se muestran dos datos:

```text
ADC promedio: XXXX.XX | Voltaje: X.XXX V
```
El valor cambia al girar el potenciómetro. Un valor ADC cercano a 0 corresponde aproximadamente a 0 V, mientras que un valor cercano a 4095 corresponde aproximadamente a 3.3 V utilizando la escala indicada.

---

<img width="1600" height="1200" alt="Actividad 1 - código" src="https://github.com/user-attachments/assets/f5ee1fe6-8277-4db0-b7f8-ca7a52004509" />
<img width="1200" height="1600" alt="Actividad 1 - montaje" src="https://github.com/user-attachments/assets/6cb10ec2-e610-4317-847f-166a2a06046a" />

</details>

**Descripción:**  
Escribe brevemente qué se observa en la fotografía y qué parte del ejercicio demuestra.

---

## Actividad 02 — Conexión WiFi con ESP32

<details>
<summary><strong>Ver enunciado</strong></summary>

La guía solicita crear una red WiFi utilizando un **Smartphone como Hotspot**, conectar el ESP32 a esa red y visualizar en el monitor serial la **dirección IP asignada**.

</details>
<details>
<summary><strong>Ver código</strong></summary>

```cpp
#include <WiFi.h>

const char* ssid = "TU_WIFI";
const char* password = "TU_PASSWORD";

void setup() {

  Serial.begin(115200);

  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println();
  Serial.println("WiFi conectado correctamente");

  Serial.print("Direccion IP del ESP32: ");
  Serial.println(WiFi.localIP());
}

void loop() {
}
```

</details>

<details>
<summary><strong>Explicación</strong></summary>

Primero se incluye la biblioteca `WiFi.h`, necesaria para manejar la conexión inalámbrica del ESP32.

Las variables `ssid` y `password` almacenan el nombre y la contraseña del Hotspot del celular. La función:

```cpp
WiFi.begin(ssid, password);
```

inicia el proceso de conexión.

Mientras el ESP32 no esté conectado, el programa permanece dentro del `while` y muestra puntos en el monitor serial. Cuando la conexión se establece, `WiFi.localIP()` permite obtener la dirección IP asignada por la red.

</details>

#### Resultado esperado

En el monitor serial debe aparecer un mensaje similar a:

```text
Conectando a: TU_WIFI
.....
WiFi conectado correctamente
Direccion IP del ESP32: 192.168.X.XXX
```
La IP concreta depende de la red creada por el Smartphone, por lo que no se debe inventar un valor específico.
<img width="1600" height="1200" alt="Actividad 2 - conexión WiFi" src="https://github.com/user-attachments/assets/00ee8635-6534-4623-af6a-12a99472b794" />

---

**Descripción:**  
Escribe brevemente qué se observa en la fotografía y qué parte del ejercicio demuestra.

---

## Actividad 03 — Envío del potenciómetro a la nube

<details>
<summary><strong>Ver enunciado</strong></summary>

La guía solicita escribir un código que muestre en tiempo real la variación del **potenciómetro conectado al ESP32** en las siguientes plataformas:

- Arduino Cloud.
- ThingSpeak.
- Ubidots.

La actividad está planteada explícitamente de esta manera en la guía oficial.

</details>

### Datos utilizados

Se mantiene el potenciómetro conectado al **GPIO 34** y se utilizan diez muestras para obtener un promedio.

| Dato | Descripción |
|---|---|
| ADC promedio | Promedio de las diez lecturas |
| Voltaje | Conversión aproximada del ADC a voltaje |
| Pin | GPIO 34 |

### 3.1 ThingSpeak

<details>
<summary><strong>Ver código</strong></summary>

```cpp
#include <WiFi.h>
#include <ThingSpeak.h>

const char* ssid = "TU_WIFI";
const char* password = "TU_PASSWORD";

unsigned long channelID = 0;
const char* writeAPIKey = "TU_WRITE_API_KEY";

WiFiClient client;

const int potPin = 34;
const int muestras = 10;

void setup() {

  Serial.begin(115200);

  WiFi.begin(ssid, password);

  Serial.print("Conectando a WiFi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println();
  Serial.println("WiFi conectado");

  ThingSpeak.begin(client);
}

void loop() {

  long suma = 0;

  // Tomar 10 muestras
  for (int i = 0; i < muestras; i++) {
    suma += analogRead(potPin);
    delay(10);
  }

  // Promedio
  float adcPromedio = suma / (float)muestras;

  // Conversión a voltaje
  float voltaje = adcPromedio * 3.3 / 4095.0;

  // Mostrar datos
  Serial.print("ADC promedio: ");
  Serial.print(adcPromedio);

  Serial.print(" | Voltaje: ");
  Serial.print(voltaje, 3);
  Serial.println(" V");

  // Enviar a ThingSpeak
  ThingSpeak.setField(1, adcPromedio);
  ThingSpeak.setField(2, voltaje);

  int respuesta = ThingSpeak.writeFields(channelID, writeAPIKey);

  if (respuesta == 200) {
    Serial.println("Datos enviados a ThingSpeak correctamente");
  } else {
    Serial.print("Error al enviar. Codigo: ");
    Serial.println(respuesta);
  }

  delay(15000);
}
```

</details>

### Configuración del canal

| Campo | Dato |
|---|---|
| Field 1 | ADC promedio |
| Field 2 | Voltaje estimado (V) |

<details>
<summary><strong>Explicación</strong></summary>

El programa conserva la lógica de la Actividad 01: toma diez muestras, calcula el promedio y convierte el resultado a voltaje. Después utiliza `ThingSpeak.setField()` para colocar los datos en los campos del canal y `ThingSpeak.writeFields()` para enviarlos.

La espera de 15 segundos permite realizar envíos separados al canal.

<details>
<summary><strong>Resultado</strong></summary>

En ThingSpeak se deben observar las variaciones de:

- ADC promedio.
- Voltaje estimado.

Al mover el potenciómetro, los valores enviados cambian y las gráficas del canal muestran esa variación.

---

</details>

</details>

### 3.2 Arduino Cloud

### Implementación

Arduino Cloud utiliza variables creadas desde la plataforma. Para este ejercicio se pueden crear, por ejemplo:

| Variable | Tipo | Uso |
|---|---|---|
| `potValue` | `int` | Lectura ADC |
| `potVoltage` | `float` | Voltaje estimado |

El archivo `thingProperties.h` es generado por Arduino Cloud a partir de la configuración del dispositivo y las variables.

<details>
<summary><strong>Ver código principal</strong></summary>

```cpp
#include "thingProperties.h"

const int potPin = 34;

void setup() {

  Serial.begin(115200);
  delay(1500);

  initProperties();

  ArduinoCloud.begin(ArduinoIoTPreferredConnection);

  setDebugMessageLevel(2);
  ArduinoCloud.printDebugInfo();
}

void loop() {

  ArduinoCloud.update();

  potValue = analogRead(potPin);

  potVoltage = potValue * 3.3 / 4095.0;

  Serial.print("ADC: ");
  Serial.print(potValue);

  Serial.print(" | Voltaje: ");
  Serial.print(potVoltage, 3);
  Serial.println(" V");

  delay(500);
}
```

</details>

<details>
<summary><strong>Explicación</strong></summary>

`ArduinoCloud.update()` mantiene la comunicación entre el ESP32 y Arduino Cloud. En cada ciclo se actualiza la variable `potValue` con la lectura del potenciómetro y `potVoltage` con la conversión correspondiente.

> **Nota:** la guía oficial proporciona un tutorial de conexión a Arduino Cloud, pero no incluye dentro del documento el contenido completo del archivo generado `thingProperties.h`. Por ello, ese archivo debe ser generado automáticamente desde la cuenta de Arduino Cloud y no se inventa aquí.

<details>
<summary><strong>Resultado</strong></summary>

El dashboard de Arduino Cloud debe mostrar la variación de las variables `potValue` y `potVoltage` mientras se mueve el potenciómetro.

---

</details>

</details>

### 3.3 Ubidots

### Implementación

La guía oficial incluye un tutorial específico para conectar un ESP32 a Ubidots mediante MQTT.

Para esta parte se debe configurar en Ubidots:

- Token de acceso.
- Nombre del dispositivo.
- Variable donde se almacenará la lectura.
- Broker y credenciales correspondientes.

### Estructura de datos

```text
ESP32
  ↓
Lectura del potenciómetro
  ↓
Promedio ADC
  ↓
MQTT
  ↓
Ubidots
  ↓
Dashboard
```
<img width="771" height="477" alt="Actividad 3 - Ubidots" src="https://github.com/user-attachments/assets/06fb406a-e6dc-47d7-add6-9211baee8d5a" />
<img width="1600" height="1085" alt="Actividad 3 - potenciómetro" src="https://github.com/user-attachments/assets/e1ea22e4-f89c-42a1-964e-49dd2a5a9e9d" />

---

**Descripción:**  
Escribe brevemente qué se observa en la fotografía y qué parte del ejercicio demuestra.

---

## Actividad 04 — Sensor del kit Keystudio en la nube

<details>
<summary><strong>Ver enunciado</strong></summary>

La actividad solicita conectar al ESP32 uno de los sensores del kit Keystudio, por ejemplo **LM35 o LDR**, y mostrar en tiempo real su variación en:

- Arduino Cloud.
- ThingSpeak.
- Ubidots.

Esto corresponde directamente a la Actividad 04 de la guía oficial.

</details>

### Sensor seleccionado: LDR

Para mantener la continuidad con el montaje utilizado, se emplea un **LDR**.

La lectura se realiza mediante el ADC y se calcula un porcentaje relativo:

\[
\text{Porcentaje} =
\frac{\text{ADC}}{4095}\times100
\]
Este porcentaje es una normalización de la señal y **no representa una medición calibrada en lux**.

---

### 4.1 ThingSpeak

<details>
<summary><strong>Ver código</strong></summary>

```cpp
#include <WiFi.h>
#include <ThingSpeak.h>

const char* ssid = "TU_WIFI";
const char* password = "TU_PASSWORD";

unsigned long channelID = 0;
const char* writeAPIKey = "TU_WRITE_API_KEY";

WiFiClient client;

const int ldrPin = 34;
const int muestras = 10;

void setup() {

  Serial.begin(115200);

  analogReadResolution(12);

  WiFi.begin(ssid, password);

  Serial.print("Conectando a WiFi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println();
  Serial.println("WiFi conectado");

  ThingSpeak.begin(client);
}

void loop() {

  long suma = 0;

  // Tomar 10 lecturas
  for (int i = 0; i < muestras; i++) {
    suma += analogRead(ldrPin);
    delay(10);
  }

  // Calcular promedio
  float valorLDR = suma / (float)muestras;

  // Normalizar a porcentaje
  float porcentajeLuz = (valorLDR / 4095.0) * 100.0;

  Serial.print("LDR: ");
  Serial.print(valorLDR);

  Serial.print(" | Luz: ");
  Serial.print(porcentajeLuz, 1);
  Serial.println(" %");

  // Enviar a ThingSpeak
  ThingSpeak.setField(1, valorLDR);
  ThingSpeak.setField(2, porcentajeLuz);

  int respuesta = ThingSpeak.writeFields(channelID, writeAPIKey);

  if (respuesta == 200) {
    Serial.println("Datos enviados correctamente");
  } else {
    Serial.print("Error: ");
    Serial.println(respuesta);
  }

  delay(15000);
}
```

</details>

### Configuración del canal

| Campo | Dato |
|---|---|
| Field 1 | Lectura promedio del LDR |
| Field 2 | Porcentaje relativo |

<details>
<summary><strong>Explicación</strong></summary>

El programa toma diez lecturas del LDR y obtiene su promedio. Luego transforma el valor de 0–4095 a una escala porcentual de 0–100 %.

Finalmente, ambos valores se envían a ThingSpeak.

<details>
<summary><strong>Resultado</strong></summary>

La gráfica de ThingSpeak debe mostrar variaciones cuando cambia la iluminación que recibe el LDR.

En una prueba documentada en el material de referencia aparecen valores como **3428.30 (83.7 %)** y **1062.30 (25.9 %)**. Estos valores corresponden a la normalización utilizada y no deben interpretarse como lux.

---

</details>

</details>

### 4.2 Arduino Cloud

### Variables

| Variable | Tipo | Descripción |
|---|---|---|
| `ldrValue` | `int` | Lectura ADC del LDR |
| `ldrPercent` | `float` | Porcentaje relativo |

<details>
<summary><strong>Ver código principal</strong></summary>

```cpp
#include "thingProperties.h"

const int ldrPin = 34;

void setup() {

  Serial.begin(115200);
  delay(1500);

  initProperties();

  ArduinoCloud.begin(ArduinoIoTPreferredConnection);

  setDebugMessageLevel(2);
  ArduinoCloud.printDebugInfo();
}

void loop() {

  ArduinoCloud.update();

  ldrValue = analogRead(ldrPin);

  ldrPercent = (ldrValue / 4095.0) * 100.0;

  Serial.print("LDR: ");
  Serial.print(ldrValue);

  Serial.print(" | Porcentaje: ");
  Serial.print(ldrPercent, 1);
  Serial.println(" %");

  delay(500);
}
```

</details>

<details>
<summary><strong>Explicación</strong></summary>

La lectura del LDR se almacena en `ldrValue`. Después se calcula el porcentaje relativo y se almacena en `ldrPercent`. `ArduinoCloud.update()` mantiene sincronizados los datos con la plataforma.

---

</details>

### 4.3 Ubidots

La guía oficial incluye un tutorial de conexión del ESP32 a Ubidots mediante MQTT. Para completar esta parte deben configurarse las credenciales de la cuenta y la variable del dispositivo.

<img width="575" height="395" alt="Actividad 4 - resultados" src="https://github.com/user-attachments/assets/cecbd04f-f1dc-4b55-9e3a-e6adf1e1f720" />
<img width="650" height="572" alt="Actividad 4 - sensor LDR" src="https://github.com/user-attachments/assets/7e7ad228-2bc7-4b95-a2aa-b6b6c468f9d9" />

---

**Descripción:**  
Escribe brevemente qué se observa en la fotografía y qué parte del ejercicio demuestra.

---

## Actividad 05 — Control de un LED desde la web

<details>
<summary><strong>Ver enunciado</strong></summary>

La guía solicita conectar un LED a uno de los pines digitales del ESP32 y controlar su encendido desde una plataforma web de preferencia.

Para esta implementación se utiliza un **servidor web creado directamente por el ESP32**, utilizando la librería `WebServer.h`.

</details>

### Conexión

| Componente | ESP32 |
|---|---|
| Ánodo del LED mediante resistencia | GPIO 23 |
| Cátodo | GND |

<details>
<summary><strong>Ver código</strong></summary>

```cpp
#include <WiFi.h>
#include <WebServer.h>

const char* ssid = "TU_WIFI";
const char* password = "TU_PASSWORD";

const int ledPin = 23;

WebServer server(80);

String pagina() {

  String html = R"rawliteral(
  <!DOCTYPE html>
  <html>

  <head>
    <meta charset="UTF-8">

    <meta name="viewport"
    content="width=device-width, initial-scale=1.0">

    <title>ESP32 - Control Web</title>

    <style>

      body {
        font-family: Arial, sans-serif;
        text-align: center;
        margin-top: 50px;
      }

      h1 {
        color: #333;
      }

      button {
        width: 200px;
        padding: 15px;
        margin: 10px;
        border: none;
        border-radius: 8px;
        color: white;
        font-size: 18px;
        cursor: pointer;
      }

      .encender {
        background-color: green;
      }

      .apagar {
        background-color: red;
      }

    </style>

  </head>

  <body>

    <h1>ESP32 - Control Web</h1>

    <h2>Actividad 05 - IoT</h2>

    <p>Control del LED externo conectado al GPIO 23</p>

    <p>
      <a href="/encender">
        <button class="encender">
          ENCENDER LED
        </button>
      </a>
    </p>

    <p>
      <a href="/apagar">
        <button class="apagar">
          APAGAR LED
        </button>
      </a>
    </p>

  </body>

  </html>
  )rawliteral";

  return html;
}

void inicio() {

  server.send(
    200,
    "text/html",
    pagina()
  );

}

void encenderLED() {

  digitalWrite(ledPin, HIGH);

  Serial.println("LED encendido");

  server.send(
    200,
    "text/html",
    pagina()
  );

}

void apagarLED() {

  digitalWrite(ledPin, LOW);

  Serial.println("LED apagado");

  server.send(
    200,
    "text/html",
    pagina()
  );

}

void setup() {

  Serial.begin(115200);

  pinMode(ledPin, OUTPUT);

  digitalWrite(ledPin, LOW);

  Serial.println("Conectando al WiFi...");

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {

    delay(500);

    Serial.print(".");

  }

  Serial.println();

  Serial.println(
    "WiFi conectado correctamente"
  );

  Serial.print(
    "Direccion IP del ESP32: "
  );

  Serial.println(
    WiFi.localIP()
  );

  server.on(
    "/",
    inicio
  );

  server.on(
    "/encender",
    encenderLED
  );

  server.on(
    "/apagar",
    apagarLED
  );

  server.begin();

  Serial.println(
    "Servidor web iniciado"
  );

}

void loop() {

  server.handleClient();

}
```

</details>

<details>
<summary><strong>Explicación</strong></summary>

El ESP32 se conecta primero a la red WiFi configurada. Después inicia un servidor web en el puerto 80.

Se crean tres rutas:

- `/` → muestra la página.
- `/encender` → coloca el GPIO 23 en `HIGH`.
- `/apagar` → coloca el GPIO 23 en `LOW`.

Cuando el usuario presiona uno de los botones, el navegador realiza una solicitud al ESP32 y el servidor ejecuta la función correspondiente.

</details>

### Procedimiento

1. Conectar el LED al GPIO 23 mediante una resistencia.
2. Colocar el nombre de la red WiFi en `TU_WIFI`.
3. Colocar la contraseña en `TU_PASSWORD`.
4. Cargar el programa al ESP32.
5. Abrir el monitor serial a **115200 baudios**.
6. Esperar hasta que aparezca la dirección IP.
7. Conectar el computador o celular a la misma red.
8. Introducir la IP del ESP32 en el navegador.
9. Presionar **ENCENDER LED** o **APAGAR LED**.

---

<img width="647" height="827" alt="Actividad 5 - control LED" src="https://github.com/user-attachments/assets/dfcc0cb8-42b0-4bc9-8767-c8cd63c6b282" />
<img width="782" height="586" alt="Actividad 5 - interfaz web" src="https://github.com/user-attachments/assets/17b4080e-526d-4345-aee0-c9636ced4cc6" />

**Descripción:**  
Escribe brevemente qué se observa en la fotografía y qué parte del ejercicio demuestra.

---

## Conclusiones

El taller permitió aplicar los conceptos básicos de IoT mediante la adquisición de señales con un ESP32, su procesamiento y posterior comunicación mediante WiFi.

En la primera actividad se aplicó un promedio de lecturas del ADC y una conversión aproximada a voltaje. En la segunda se estableció la comunicación WiFi y se identificó la dirección IP del dispositivo.

Las actividades 03 y 04 extendieron el proceso hacia plataformas IoT, permitiendo visualizar los datos del potenciómetro y del sensor LDR en la nube. Finalmente, en la Actividad 05 se utilizó el ESP32 como servidor web para controlar un LED remotamente desde un navegador conectado a la misma red.

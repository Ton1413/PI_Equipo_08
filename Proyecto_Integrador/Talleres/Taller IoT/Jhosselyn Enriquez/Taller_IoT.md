# Taller de Internet de las Cosas (IoT) con ESP32

## Introducción

En este taller se trabajó con un **ESP32** para realizar adquisición de datos, conexión WiFi, envío de información a plataformas IoT y control de un actuador desde una interfaz web. La guía oficial plantea cinco actividades principales: mejorar la lectura de un potenciómetro, conectarse a una red WiFi, enviar datos a la nube, enviar datos de un sensor del kit Keystudio y controlar un LED desde una plataforma web. fileciteturn0file0L69-L75

El desarrollo toma como referencia el código y la estructura del material compartido, pero se adapta a los requerimientos concretos de la guía oficial. La guía también indica que se puede utilizar ChatGPT para apoyar la programación. fileciteturn0file0L71-L75

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

La guía oficial considera, entre otros materiales, un ESP32 Dev Kit 1, un Arduino Explore IoT Kit, un kit Keystudio 48 en 1, un multímetro y una protoboard. fileciteturn0file0L19-L24

---

# Actividad 01 — Lectura del potenciómetro, promedio y voltaje

## Enunciado

La actividad solicita mejorar el código básico de lectura del potenciómetro mediante un **promediado de los datos** y convertir los valores obtenidos por el ADC a **valores de voltaje**. fileciteturn0file0L69-L75

## Conexión

| Terminal del potenciómetro | ESP32 |
|---|---|
| Extremo | 3.3 V |
| Otro extremo | GND |
| Terminal central | GPIO 34 |

## Código

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

## Explicación

El ESP32 realiza diez lecturas consecutivas del potenciómetro mediante `analogRead()`. Estas lecturas se acumulan en `suma` y posteriormente se dividen entre el número de muestras para obtener el promedio.

La conversión utilizada es:

\[
V = \frac{ADC \times 3.3}{4095}
\]

donde `ADC` corresponde al valor promedio obtenido, `3.3` V es la referencia utilizada en el ejercicio y `4095` corresponde al máximo considerado para una lectura ADC de 12 bits.

El promedio permite obtener una lectura más estable que una única medición.

## Resultado

En el monitor serial se muestran dos datos:

```text
ADC promedio: XXXX.XX | Voltaje: X.XXX V
```

El valor cambia al girar el potenciómetro. Un valor ADC cercano a 0 corresponde aproximadamente a 0 V, mientras que un valor cercano a 4095 corresponde aproximadamente a 3.3 V utilizando la escala indicada.

> **Nota:** esta conversión representa una aproximación de escala y no una calibración física del ADC.

---

# Actividad 02 — Conexión WiFi con ESP32

## Enunciado

La guía solicita crear una red WiFi utilizando un **Smartphone como Hotspot**, conectar el ESP32 a esa red y visualizar en el monitor serial la **dirección IP asignada**. fileciteturn0file0L101-L107

## Código

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

## Explicación

Primero se incluye la biblioteca `WiFi.h`, necesaria para manejar la conexión inalámbrica del ESP32.

Las variables `ssid` y `password` almacenan el nombre y la contraseña del Hotspot del celular. La función:

```cpp
WiFi.begin(ssid, password);
```

inicia el proceso de conexión.

Mientras el ESP32 no esté conectado, el programa permanece dentro del `while` y muestra puntos en el monitor serial. Cuando la conexión se establece, `WiFi.localIP()` permite obtener la dirección IP asignada por la red.

## Resultado esperado

En el monitor serial debe aparecer un mensaje similar a:

```text
Conectando a: TU_WIFI
.....
WiFi conectado correctamente
Direccion IP del ESP32: 192.168.X.XXX
```

La IP concreta depende de la red creada por el Smartphone, por lo que no se debe inventar un valor específico.

---

# Actividad 03 — Envío del potenciómetro a la nube

## Enunciado

La guía solicita escribir un código que muestre en tiempo real la variación del **potenciómetro conectado al ESP32** en las siguientes plataformas:

- Arduino Cloud.
- ThingSpeak.
- Ubidots.

La actividad está planteada explícitamente de esta manera en la guía oficial. fileciteturn0file0L160-L166

## Datos utilizados

Se mantiene el potenciómetro conectado al **GPIO 34** y se utilizan diez muestras para obtener un promedio.

| Dato | Descripción |
|---|---|
| ADC promedio | Promedio de las diez lecturas |
| Voltaje | Conversión aproximada del ADC a voltaje |
| Pin | GPIO 34 |

---

## 3.1 ThingSpeak

### Código

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

### Configuración del canal

| Campo | Dato |
|---|---|
| Field 1 | ADC promedio |
| Field 2 | Voltaje estimado (V) |

### Explicación

El programa conserva la lógica de la Actividad 01: toma diez muestras, calcula el promedio y convierte el resultado a voltaje. Después utiliza `ThingSpeak.setField()` para colocar los datos en los campos del canal y `ThingSpeak.writeFields()` para enviarlos.

La espera de 15 segundos permite realizar envíos separados al canal.

### Resultado

En ThingSpeak se deben observar las variaciones de:

- ADC promedio.
- Voltaje estimado.

Al mover el potenciómetro, los valores enviados cambian y las gráficas del canal muestran esa variación.

---

## 3.2 Arduino Cloud

### Implementación

Arduino Cloud utiliza variables creadas desde la plataforma. Para este ejercicio se pueden crear, por ejemplo:

| Variable | Tipo | Uso |
|---|---|---|
| `potValue` | `int` | Lectura ADC |
| `potVoltage` | `float` | Voltaje estimado |

El archivo `thingProperties.h` es generado por Arduino Cloud a partir de la configuración del dispositivo y las variables.

### Código principal

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

### Explicación

`ArduinoCloud.update()` mantiene la comunicación entre el ESP32 y Arduino Cloud. En cada ciclo se actualiza la variable `potValue` con la lectura del potenciómetro y `potVoltage` con la conversión correspondiente.

> **Nota:** la guía oficial proporciona un tutorial de conexión a Arduino Cloud, pero no incluye dentro del documento el contenido completo del archivo generado `thingProperties.h`. Por ello, ese archivo debe ser generado automáticamente desde la cuenta de Arduino Cloud y no se inventa aquí.

### Resultado

El dashboard de Arduino Cloud debe mostrar la variación de las variables `potValue` y `potVoltage` mientras se mueve el potenciómetro.

---

## 3.3 Ubidots

### Implementación

La guía oficial incluye un tutorial específico para conectar un ESP32 a Ubidots mediante MQTT. fileciteturn0file0L149-L153

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

### Resultado esperado

El valor del potenciómetro debe actualizarse en la variable configurada en Ubidots y visualizarse en su dashboard.

> **Nota:** la guía proporciona el tutorial de conexión, pero no contiene credenciales ni un código completo específico para una cuenta de Ubidots. Por seguridad, las credenciales deben colocarse únicamente en la copia local del programa y no publicarse en el `.md`.

---

# Actividad 04 — Sensor del kit Keystudio en la nube

## Enunciado

La actividad solicita conectar al ESP32 uno de los sensores del kit Keystudio, por ejemplo **LM35 o LDR**, y mostrar en tiempo real su variación en:

- Arduino Cloud.
- ThingSpeak.
- Ubidots.

Esto corresponde directamente a la Actividad 04 de la guía oficial. fileciteturn0file0L168-L174

## Sensor seleccionado: LDR

Para mantener la continuidad con el montaje utilizado, se emplea un **LDR**.

La lectura se realiza mediante el ADC y se calcula un porcentaje relativo:

\[
\text{Porcentaje} =
\frac{\text{ADC}}{4095}\times100
\]

Este porcentaje es una normalización de la señal y **no representa una medición calibrada en lux**.

---

## 4.1 ThingSpeak

### Código

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

### Configuración del canal

| Campo | Dato |
|---|---|
| Field 1 | Lectura promedio del LDR |
| Field 2 | Porcentaje relativo |

### Explicación

El programa toma diez lecturas del LDR y obtiene su promedio. Luego transforma el valor de 0–4095 a una escala porcentual de 0–100 %.

Finalmente, ambos valores se envían a ThingSpeak.

### Resultado

La gráfica de ThingSpeak debe mostrar variaciones cuando cambia la iluminación que recibe el LDR.

En una prueba documentada en el material de referencia aparecen valores como **3428.30 (83.7 %)** y **1062.30 (25.9 %)**. Estos valores corresponden a la normalización utilizada y no deben interpretarse como lux.

---

## 4.2 Arduino Cloud

### Variables

| Variable | Tipo | Descripción |
|---|---|---|
| `ldrValue` | `int` | Lectura ADC del LDR |
| `ldrPercent` | `float` | Porcentaje relativo |

### Código principal

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

### Explicación

La lectura del LDR se almacena en `ldrValue`. Después se calcula el porcentaje relativo y se almacena en `ldrPercent`. `ArduinoCloud.update()` mantiene sincronizados los datos con la plataforma.

---

## 4.3 Ubidots

La guía oficial incluye un tutorial de conexión del ESP32 a Ubidots mediante MQTT. Para completar esta parte deben configurarse las credenciales de la cuenta y la variable del dispositivo.

La estructura de funcionamiento es:

```text
LDR
 ↓
ESP32
 ↓
Lectura ADC
 ↓
Porcentaje relativo
 ↓
MQTT
 ↓
Ubidots
 ↓
Dashboard
```

### Resultado esperado

El dashboard debe mostrar la variación de la lectura del LDR en tiempo real.

> **Importante:** el porcentaje calculado representa una escala relativa de la señal ADC. No es una medición de iluminancia en lux.

---

# Actividad 05 — Control de un LED desde la web

## Enunciado

La guía solicita conectar un LED a uno de los pines digitales del ESP32 y controlar su encendido desde una plataforma web de preferencia. fileciteturn0file0L176-L181

Para esta implementación se utiliza un **servidor web creado directamente por el ESP32**, utilizando la librería `WebServer.h`.

## Conexión

| Componente | ESP32 |
|---|---|
| Ánodo del LED mediante resistencia | GPIO 23 |
| Cátodo | GND |

## Código

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

## Explicación

El ESP32 se conecta primero a la red WiFi configurada. Después inicia un servidor web en el puerto 80.

Se crean tres rutas:

- `/` → muestra la página.
- `/encender` → coloca el GPIO 23 en `HIGH`.
- `/apagar` → coloca el GPIO 23 en `LOW`.

Cuando el usuario presiona uno de los botones, el navegador realiza una solicitud al ESP32 y el servidor ejecuta la función correspondiente.

## Procedimiento

1. Conectar el LED al GPIO 23 mediante una resistencia.
2. Colocar el nombre de la red WiFi en `TU_WIFI`.
3. Colocar la contraseña en `TU_PASSWORD`.
4. Cargar el programa al ESP32.
5. Abrir el monitor serial a **115200 baudios**.
6. Esperar hasta que aparezca la dirección IP.
7. Conectar el computador o celular a la misma red.
8. Introducir la IP del ESP32 en el navegador.
9. Presionar **ENCENDER LED** o **APAGAR LED**.

## Resultado esperado

La página web debe mostrar dos botones:

```text
ESP32 - Control Web

Actividad 05 - IoT

[ ENCENDER LED ]

[ APAGAR LED ]
```

Al presionar **ENCENDER LED**, el LED conectado al GPIO 23 debe encenderse. Al presionar **APAGAR LED**, debe apagarse.

---

# Evidencias

Las evidencias deben colocarse junto a la actividad correspondiente.

### Actividad 01

Colocar una fotografía del montaje del potenciómetro y, si se dispone de ella, una captura del monitor serial mostrando el promedio y el voltaje.

### Actividad 02

Colocar una captura del monitor serial donde se observe la conexión WiFi y la IP asignada.

### Actividad 03

Colocar las capturas de los dashboards utilizados:

- Arduino Cloud.
- ThingSpeak.
- Ubidots.

### Actividad 04

Colocar las capturas correspondientes al sensor LDR:

- Arduino Cloud.
- ThingSpeak.
- Ubidots.

### Actividad 05

Colocar una captura de la página web con los botones de control y una fotografía del circuito del LED.

---

# Preparación de los programas

1. Abrir Arduino IDE.
2. Seleccionar la placa ESP32 utilizada.
3. Seleccionar el puerto correspondiente.
4. Instalar el soporte de placas ESP32 si todavía no está instalado.
5. Instalar `ThingSpeak` para las actividades que utilicen esta plataforma.
6. Copiar el código correspondiente a un nuevo sketch.
7. Reemplazar `TU_WIFI` y `TU_PASSWORD` por las credenciales reales.
8. En ThingSpeak, reemplazar `channelID` y `TU_WRITE_API_KEY` por los datos del canal.
9. En Arduino Cloud, configurar el dispositivo y las variables desde la plataforma para que se genere `thingProperties.h`.
10. En Ubidots, configurar el dispositivo, las variables y las credenciales correspondientes.
11. Cargar el programa al ESP32.
12. Abrir el monitor serial a **115200 baudios**.

Las credenciales utilizadas en los códigos son únicamente marcadores de posición y no deben publicarse en el documento final.

---

# Comparación con la guía oficial

| Actividad | Lo solicitado por la guía | Adaptación realizada |
|---|---|---|
| 01 | Promediar datos y convertir ADC a voltaje | Se utilizan 10 muestras y conversión a voltaje |
| 02 | Conectar ESP32 al Hotspot y mostrar IP | Se utiliza `WiFi.begin()` y `WiFi.localIP()` |
| 03 | Potenciómetro en Arduino Cloud, ThingSpeak y Ubidots | Se incluye la lógica del potenciómetro para las tres plataformas |
| 04 | Sensor Keystudio en Arduino Cloud, ThingSpeak y Ubidots | Se selecciona LDR y se calcula una escala porcentual |
| 05 | Controlar LED desde una plataforma web | Se implementa un servidor web en el ESP32 |

La diferencia principal respecto al material de referencia es que el código del compañero desarrollaba directamente ThingSpeak para las actividades 03 y 04, mientras que la guía oficial exige **Arduino Cloud, ThingSpeak y Ubidots**. Por ello, en esta versión se amplió la estructura para contemplar las tres plataformas. La guía oficial especifica explícitamente estas tres plataformas para ambas actividades. fileciteturn0file0L160-L174

---

# Conclusiones

El taller permitió aplicar los conceptos básicos de IoT mediante la adquisición de señales con un ESP32, su procesamiento y posterior comunicación mediante WiFi.

En la primera actividad se aplicó un promedio de lecturas del ADC y una conversión aproximada a voltaje. En la segunda se estableció la comunicación WiFi y se identificó la dirección IP del dispositivo.

Las actividades 03 y 04 extendieron el proceso hacia plataformas IoT, permitiendo visualizar los datos del potenciómetro y del sensor LDR en la nube. Finalmente, en la Actividad 05 se utilizó el ESP32 como servidor web para controlar un LED remotamente desde un navegador conectado a la misma red.

En conjunto, las actividades muestran el flujo básico de un sistema IoT:

```text
Sensor / Actuador
       ↓
      ESP32
       ↓
Procesamiento de datos
       ↓
      WiFi
       ↓
Plataforma IoT / Servidor web
       ↓
Visualización o control
```

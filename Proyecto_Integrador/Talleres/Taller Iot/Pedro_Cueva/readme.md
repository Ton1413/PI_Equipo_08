# Taller de Internet (IoT) con ESP32

En este taller trabajé con un ESP32 para leer un potenciómetro, explorar redes WiFi, enviar mediciones a ThingSpeak y controlar un LED desde una página web. También utilicé un sensor de luz LDR para observar cambios de iluminación.

## Materiales y herramientas

- ESP32, cable USB, protoboard y cables de conexión.
- Potenciómetro, LDR, LED y resistencias para los circuitos.
- Arduino IDE, conexión WiFi y un canal de ThingSpeak.
- Librerías `WiFi.h`, `WebServer.h` y `ThingSpeak.h` según la actividad.

## Actividad 01 — Lectura del potenciómetro, promedio y voltaje

Conecté el potenciómetro para observar su lectura analógica en el monitor serial. El código de lectura básica utiliza el GPIO 34 y muestra una lectura cada 500 ms.



<img width="1280" height="960" alt="03_potenciometro_nube" src="https://github.com/user-attachments/assets/94fd2cef-6c87-4d2b-9654-8a7882d3cca7" />


La fotografía corresponde a la **lectura básica**. La versión de promedio y voltaje se recuperó del código compartido durante el taller; sus resultados también aparecen en la prueba posterior con ThingSpeak.

### Conexión de referencia

| Terminal del potenciómetro | ESP32 |
|---|---|
| Extremo | 3.3 V |
| Otro extremo | GND |
| Central | GPIO 34 |

La versión ampliada suma 10 lecturas y divide la suma entre 10. Después estima el voltaje con `voltaje = promedio × 3.3 / 4095`. Esta conversión es una aproximación de escala, no una calibración del ADC.


### Código

```cpp
const int potPin = 34;        // Pin del potenciómetro
const int muestras = 10;      // Número de lecturas para promediar
const float Vref = 3.3;       // Voltaje de referencia del ESP32
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

  // Calcular promedio
  float promedio = suma / (float)muestras;

  // Convertir ADC a voltaje
  float voltaje = promedio * Vref / ADCmax;

  // Mostrar resultados
  Serial.print("ADC promedio: ");
  Serial.print(promedio);

  Serial.print("   Voltaje: ");
  Serial.print(voltaje, 3);
  Serial.println(" V");

  delay(500);
}
```

### Código de lectura básica mostrado en la foto

```cpp
const int potPin = 34;
void setup() { Serial.begin(115200); }
void loop() {
  int valor = analogRead(potPin);
  Serial.println(valor);
  delay(500);
}
```

## Actividad 02 — WiFi: exploración de redes y conexión

La evidencia disponible muestra un **escaneo de redes WiFi**: el monitor serial enumera las redes detectadas y su intensidad de señal. El programa visible utiliza `WiFi.scanNetworks()`.


<img width="960" height="1280" alt="02_escaneo_wifi" src="https://github.com/user-attachments/assets/72cd5cd6-9b7b-4071-94a6-7e4ed34a2f79" />


Como parte del material del taller, se incluye también el programa de conexión WiFi recuperado de la conversación. Este programa intenta conectarse a la red configurada y muestra la IP mediante `WiFi.localIP()` cuando lo consigue. La fotografía anterior documenta el escaneo; no es una captura de esa IP.


### Código

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
  Serial.println("¡WiFi conectado!");

  Serial.print("Dirección IP del ESP32: ");
  Serial.println(WiFi.localIP());
}

void loop() {

}
```

## Actividad 03 — Potenciómetro y ThingSpeak

Utilicé el potenciómetro en GPIO 34 para enviar el promedio del ADC y su voltaje estimado a ThingSpeak. El programa toma 10 muestras y envía ambos resultados al mismo canal.

| Campo | Dato |
|---|---|
| Field 1 | ADC promedio |
| Field 2 | Voltaje estimado, en V |


<img width="960" height="1280" alt="01_potenciometro" src="https://github.com/user-attachments/assets/2f9631f3-e7b9-45b9-a115-40e1b7672fb9" />


Las fotografías originales del monitor muestran mensajes de envío correcto y lecturas de hasta 4095, equivalentes a 3.300 V con la fórmula utilizada. En el código hay una espera de 15 segundos después de cada envío, además del tiempo de lectura y comunicación.


### Código

```cpp
#include <WiFi.h>
#include <ThingSpeak.h>

const char* ssid = "TU_WIFI";
const char* password = "TU_PASSWORD";

unsigned long channelID = 0; // Reemplazar por el ID numerico de tu canal
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

  // Calcular promedio
  float adcPromedio = suma / (float)muestras;

  // Convertir ADC a voltaje
  float voltaje = adcPromedio * 3.3 / 4095.0;

  // Mostrar en monitor serie
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

## Actividad 04 — Sensor de luz LDR con ThingSpeak

Para esta actividad utilicé un **LDR**, tal como se observa en el montaje. El código lee el GPIO 34, promedia 10 muestras y calcula un porcentaje relativo mediante `valorLDR / 4095 × 100`.

| Campo | Dato |
|---|---|
| Field 1 | Lectura promedio del LDR |
| Field 2 | Porcentaje relativo de la lectura ADC |


<img width="1280" height="960" alt="04_ldr_nube" src="https://github.com/user-attachments/assets/e27b4824-b6ab-4aed-bf4b-f92067e4446f" />


Las gráficas muestran variaciones durante la prueba. En el monitor de las fotografías originales aparecen, por ejemplo, una lectura de 3428.30 y 83.7 %, seguida de valores cercanos a 1062.30 y 25.9 %.

El porcentaje es una normalización de la señal: **no representa una medición calibrada en lux**. La relación entre más luz y mayor o menor lectura depende de la disposición del LDR y la resistencia en el divisor. Las fotos no permiten confirmar el valor de esa resistencia; por eso no se asigna un valor al montaje documentado.


### Código

```cpp
#include <WiFi.h>
#include <ThingSpeak.h>

const char* ssid = "TU_WIFI";
const char* password = "TU_PASSWORD";

unsigned long channelID = 0; // Reemplazar por el ID numerico de tu canal
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

  // Promedio
  float valorLDR = suma / (float)muestras;

  // Convertir de 0-4095 a porcentaje
  float porcentajeLuz = (valorLDR / 4095.0) * 100.0;

  Serial.print("LDR: ");
  Serial.print(valorLDR);

  Serial.print(" | Luz: ");
  Serial.print(porcentajeLuz, 1);
  Serial.println(" %");

  // ThingSpeak
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

## Actividad 05 — Control de un LED desde la web

Utilicé el ESP32 como servidor web para controlar un LED externo conectado al GPIO 23. La página tiene botones para encenderlo y apagarlo. El navegador envía las solicitudes `/encender` y `/apagar`, y el programa cambia el estado del pin.

### Aquí va la imagen

**Foto original:** `WhatsApp Image 2026-10-01 at 19.39.47.jpeg`  
**Cómo reconocerla:** Página con los botones ENCENDER LED y APAGAR LED junto al circuito.  
**Dónde colocarla:** carpeta `imagenes`, con el nombre `05_led_web.jpg`.

<img width="1280" height="960" alt="05_led_web" src="https://github.com/user-attachments/assets/d5e8a26f-21e2-42cd-87d5-1073e9e1d892" />



**Archivo original:** `WhatsApp Video 2026-10-01 at 19.39.47.mp4`. Colócalo en `videos` con el nombre `control_led.mp4`.



https://github.com/user-attachments/assets/349d789e-e085-4fb3-a8db-aa03713f4059



Para utilizarlo, el ESP32 y el equipo que abre la página deben estar conectados a la misma red. Después de cargar el programa, se consulta la IP en el monitor serial y se abre esa dirección en el navegador.

Este es el programa que se confirmó como funcional en la conversación del taller. Para esta entrega se sustituyeron las credenciales y se corrigió el texto de la página para indicar que el LED es externo.


### Código

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

## Preparación de los programas

1. Copiar el bloque de código de la actividad elegida en un nuevo sketch de Arduino IDE.
2. Seleccionar la placa ESP32 y el puerto correspondientes al equipo utilizado.
3. Tener instalado el soporte de placas ESP32 y, para las actividades 03 y 04, la librería ThingSpeak.
4. Sustituir `TU_WIFI` y `TU_PASSWORD` en la copia local del programa.
5. Para ThingSpeak, reemplazar `channelID = 0` por el ID numérico real y `TU_WRITE_API_KEY` por la clave de escritura; configurar los dos campos según las tablas.
6. Cargar el programa y abrir el monitor serial a **115200 baudios**.

Las credenciales son marcadores de posición. Los códigos de conexión esperan hasta conectarse; si los datos de red no son válidos, permanecen en esa espera.

## Conclusión

El taller permitió relacionar la lectura de señales analógicas con su visualización local y en la nube. El potenciómetro permitió observar cambios en el ADC, el LDR permitió registrar variaciones de iluminación y la página alojada en el ESP32 permitió controlar un LED desde el navegador.


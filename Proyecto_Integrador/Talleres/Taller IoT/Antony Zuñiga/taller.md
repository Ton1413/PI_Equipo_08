# Taller de Internet de las Cosas (IoT) con ESP32

En este taller se trabajó con un módulo ESP32 para explorar la lectura de señales analógicas, la conectividad Wi-Fi, la transmisión de telemetría a la plataforma IoT ThingSpeak y el control de actuadores desde un servidor web embebido.

## Materiales y Herramientas

- Módulo ESP32 Dev Module y cable de programación USB.
- Protoboard y cables de conexión (Jumpers).
- Potenciómetro de 10kΩ.
- Entorno de desarrollo Arduino IDE.
- Conexión a red Wi-Fi y canal configurado en ThingSpeak.

---

## Actividad 01 — Lectura del Potenciómetro, Promedio y Voltaje

Se conectó un potenciómetro al pin analógico GPIO 34 del ESP32. Para estabilizar la lectura del ADC (de 12 bits, con un rango de 0 a 4095), se implementó un promedio móvil de 10 muestras continuas y se convirtió dicho valor a una estimación de voltaje en el rango de 0 a 3.3 V.

### Conexión de Referencia

| Terminal del Potenciómetro | ESP32 |
|---|---|
| Extremo 1 | 3.3 V |
| Extremo 2 | GND |
| Pin Central (Wiper) | GPIO 34 |

### Evidencia Fotográfica

<img width="1200" height="1600" alt="WhatsApp Image 2026-09-29 at 5 58 41 PM1" src="https://github.com/user-attachments/assets/bcaa3afe-72dd-4630-b005-4bb85c3b9ec8" />
*Figura 1: Montaje físico en protoboard del potenciómetro conectado al ESP32.*
<img width="1600" height="1200" alt="WhatsApp Image 2026-09-29 at 5 58 41 PM" src="https://github.com/user-attachments/assets/05c5b2f2-c1eb-4d64-8134-d04899480698" />
*Figura 2: Salida del Monitor Serial mostrando el promedio del ADC y su equivalencia en voltaje.*

### Código de la Actividad

```cpp
const int potPin = 34;        // Pin analógico conectado al potenciómetro
const int muestras = 10;      // Número de lecturas para promediar
const float Vref = 3.3;       // Voltaje de referencia del ESP32
const int ADCmax = 4095;      // Resolución del ADC de 12 bits

void setup() {
  Serial.begin(115200);
}

void loop() {
  long suma = 0;

  // Tomar 10 muestras continuas
  for (int i = 0; i < muestras; i++) {
    suma += analogRead(potPin);
    delay(10);
  }

  // Calcular el promedio de las lecturas
  float valorPromedio = suma / (float)muestras;

  // Convertir el valor promedio a voltaje (0 - 3.3V)
  float voltaje = (valorPromedio * Vref) / ADCmax;

  // Mostrar resultados en el monitor serie
  Serial.print("ADC Promedio: ");
  Serial.print(valorPromedio);
  Serial.print("\t| Voltaje: ");
  Serial.print(voltaje, 2);
  Serial.println(" V");

  delay(500); // Esperar medio segundo antes de la siguiente medición
}
```
## Actividad 02 — Wi-Fi: Exploración de Redes Cercanas

En esta actividad se utilizó la librería `WiFi.h` para escanear las redes Wi-Fi al alcance del módulo ESP32. El programa analiza el entorno de radiofrecuencia e imprime en el Monitor Serial el nombre de la red (SSID), la intensidad de señal recibida (RSSI en dBm) y el estado del cifrado (si la red es abierta o protegida).

### Evidencia Fotográfica
<img width="1600" height="1200" alt="WhatsApp Image 2026-09-29 at 6 11 20 PM" src="https://github.com/user-attachments/assets/2f3843ac-970c-448d-bc62-e36832901431" />
*Figura: Monitor Serial mostrando las redes Wi-Fi detectadas por el ESP32 (Redmi 10A, UPCH_CENTRAL, Kuangg, Galaxy A36 5G BF8B).*

### Código de la Actividad

```cpp
#include "WiFi.h"

void setup() {
  Serial.begin(115200);

  // Configurar Wi-Fi en modo Estación y desconectar de cualquier AP previo
  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);

  Serial.println("Setup terminado");
}

void loop() {
  Serial.println("Escaneando redes Wi-Fi...");

  // WiFi.scanNetworks devuelve el número de redes encontradas
  int n = WiFi.scanNetworks();
  Serial.println("Escaneo finalizado.");

  if (n == 0) {
    Serial.println("No se encontraron redes.");
  } else {
    Serial.print(n);
    Serial.println(" redes encontradas:");
    for (int i = 0; i < n; ++i) {
      // Imprimir SSID y RSSI para cada red encontrada
      Serial.print(i + 1);
      Serial.print(": ");
      Serial.print(WiFi.SSID(i));
      Serial.print(" (");
      Serial.print(WiFi.RSSI(i));
      Serial.print(" dBm) [Cifrado: ");
      Serial.print(WiFi.encryptionType(i) == WIFI_AUTH_OPEN ? "Abierta" : "Protegida");
      Serial.println("]");
      delay(10);
    }
  }
  Serial.println("");

  // Esperar 5 segundos antes de realizar el próximo escaneo
  delay(5000);
}
```
## Actividad 03 — Potenciómetro y ThingSpeak

En esta actividad se configuró el ESP32 para conectarse a una red Wi-Fi y transmitir las lecturas analógicas del potenciómetro a la plataforma en la nube **ThingSpeak** mediante peticiones HTTP. El sistema lee el puerto GPIO 34 y envía periódicamente el valor recolectado.

### Configuración del Canal

| Parámetro | Valor Configurado |
|---|---|
| Red Wi-Fi (SSID) | `UPCH_CENTRAL` |
| Servidor ThingSpeak | `api.thingspeak.com` |
| Write API Key | `3X1UEXNNJH2VSEC5` |
| Pin del Potenciómetro | GPIO 34 |
| Intervalo de Envío | Cada 20 segundos (`20000 ms`) |

### Evidencia Fotográfica y Ubicación de Imágenes

> **Ubicación 1 — Código y Confirmación de Envío en Monitor Serial:**
<img width="1600" height="1175" alt="WhatsApp Image 2026-09-29 at 6 59 18 1" src="https://github.com/user-attachments/assets/c1faca7b-72d4-490b-9bbf-c7dbc69bd0d6" />

*Figura: Monitor Serial mostrando los mensajes de conexión Wi-Fi y confirmación "¡Dato enviado con éxito!".*

> **Ubicación 2 — Gráfica en la Plataforma ThingSpeak:**
> <img width="1600" height="1085" alt="WhatsApp Image 2026-09-29 at 6 59 18 PM" src="https://github.com/user-attachments/assets/b9650167-82e3-4d31-a60a-5a3d91d603cf" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-09-29 at 7 14 59 PM1" src="https://github.com/user-attachments/assets/afc3ad7e-76ea-4fb1-a415-a5246855fe64" />

*Figura: Panel de ThingSpeak mostrando la gráfica en tiempo real ("Field 1 Chart - Potenciometro ESP32") con la variación de lecturas del potenciómetro.*

### Código de la Actividad

```cpp
#include <WiFi.h>

// Credenciales de tu red Wi-Fi (reemplaza con el nombre y contraseña de tu red)
const char* ssid = "UPCH_CENTRAL";
const char* password = "CAYETANO2022";

// Configuración de ThingSpeak
const char* server = "api.thingspeak.com";
String apiKey = "3X1UEXNNJH2VSEC5"; // Tu Write API Key obtenida de ThingSpeak

int potPin = 34; // Pin del potenciómetro
unsigned long lastTime = 0;
unsigned long timerDelay = 20000; // Enviar datos cada 20 segundos

WiFiClient client;

void setup() {
  Serial.begin(115200);

  WiFi.begin(ssid, password);
  Serial.print("Conectando a Wi-Fi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\n¡Conectado a la red Wi-Fi!");
}

void loop() {
  // Enviar datos cada 20 segundos sin bloquear el código
  if ((millis() - lastTime) > timerDelay) {
    
    if (WiFi.status() == WL_CONNECTED) {
      int potValue = analogRead(potPin);

      Serial.print("Conectando a ThingSpeak... ");

      if (client.connect(server, 80)) {
        // Construir URL de la petición HTTP GET
        String url = "/update?api_key=" + apiKey + "&field1=" + String(potValue);

        client.print(String("GET ") + url + " HTTP/1.1\r\n" +
                     "Host: " + server + "\r\n" +
                     "Connection: close\r\n\r\n");

        Serial.println("¡Dato enviado con éxito!");
        Serial.print("Valor enviado: ");
        Serial.println(potValue);

        client.stop();
      } else {
        Serial.println("Conexión fallida.");
      }
    } else {
      Serial.println("Wi-Fi desconectado");
    }

    lastTime = millis();
  }
}
```
## Actividad 04 — Sensor de Luz LDR con ThingSpeak

En esta actividad se utilizó un **sensor de luz LDR (fotorresistencia)** conectado en un divisor de tensión al GPIO 34. El ESP32 realiza 10 lecturas consecutivas para calcular un promedio del ADC de 12 bits (0 a 4095) y convierte dicha medición a un porcentaje de iluminación de 0 a 100 %. Posteriormente, transmite ambos parámetros a dos campos (*Field 1* y *Field 2*) de un canal de ThingSpeak mediante peticiones HTTP.

### Configuración del Canal y Mapeo de Datos

| Campo | Dato Enviado | Descripción |
|---|---|---|
| **Field 1** | Lectura promedio del ADC | Valor digital del ADC (0 - 4095) |
| **Field 2** | Porcentaje de luz | Normalización relativa: `(valorLDR / 4095.0) * 100` |

---

### Evidencia Fotográfica y Ubicación de Imágenes

<img width="1600" height="1200" alt="WhatsApp Image 2026-09-29 at 7 15 00 PM" src="https://github.com/user-attachments/assets/9291f588-f7d1-4621-b9aa-14d35cdb7bcd" />

*Figura: Circuito con sensor LDR en protoboard y registro de variación de iluminación transmitido a la nube.*

---

### Código de la Actividad

```cpp
#include <WiFi.h>
#include <ThingSpeak.h>

// Credenciales de la red Wi-Fi
const char* ssid = "TU_WIFI";
const char* password = "TU_PASSWORD";

// Configuración de ThingSpeak
unsigned long channelID = 0000000; // Reemplazar con el ID numérico de tu canal
const char* writeAPIKey = "TU_WRITE_API_KEY";

WiFiClient client;

const int ldrPin = 34;    // Pin analógico conectado al LDR
const int muestras = 10;  // Cantidad de lecturas para el promedio

void setup() {
  Serial.begin(115200);

  // Configurar la resolución del ADC a 12 bits (0-4095)
  analogReadResolution(12);

  WiFi.begin(ssid, password);
  Serial.print("Conectando a Wi-Fi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\n¡Wi-Fi conectado con éxito!");

  // Inicializar la librería ThingSpeak
  ThingSpeak.begin(client);
}

void loop() {
  long suma = 0;

  // Tomar 10 lecturas consecutivas para filtrar el ruido
  for (int i = 0; i < muestras; i++) {
    suma += analogRead(ldrPin);
    delay(10);
  }

  // Calcular el valor promedio
  float valorLDR = suma / (float)muestras;

  // Convertir el promedio del ADC a un porcentaje estimado de luz (0% a 100%)
  float porcentajeLuz = (valorLDR / 4095.0) * 100.0;

  // Imprimir valores en el Monitor Serial
  Serial.print("LDR (ADC): ");
  Serial.print(valorLDR);
  Serial.print(" | Nivel de Luz: ");
  Serial.print(porcentajeLuz, 1);
  Serial.println(" %");

  // Asignar los valores a los campos de ThingSpeak
  ThingSpeak.setField(1, valorLDR);
  ThingSpeak.setField(2, porcentajeLuz);

  // Transmitir datos al canal
  int respuesta = ThingSpeak.writeFields(channelID, writeAPIKey);

  if (respuesta == 200) {
    Serial.println("Datos transmitidos a ThingSpeak correctamente.");
  } else {
    Serial.print("Error al transmitir datos. Código de respuesta HTTP: ");
    Serial.println(respuesta);
  }

  // Esperar 15 segundos entre transmisiones (límite recomendado de ThingSpeak)
  delay(15000);
}
```
## Actividad 05 — Control de un LED desde la web

Utilicé el ESP32 como servidor web para controlar un LED externo conectado al GPIO 23. La página aloja una interfaz HTML con botones para encenderlo y apagarlo. El navegador envía las solicitudes HTTP `/encender` y `/apagar`, y el programa cambia el estado del pin correspondientemente.

### Evidencia Fotográfica y Video

> **Ubicación de la Imagen:**
<img width="1280" height="960" alt="WhatsApp Image 2026-10-01 at 7 39 47 PM" src="https://github.com/user-attachments/assets/a5fa4e0a-fb4d-468a-8ee6-52aef63c4a82" />

*Figura: Interfaz de control web del ESP32 con botones "ENCENDER LED" y "APAGAR LED" junto al circuito en protoboard[cite: 9].*


https://github.com/user-attachments/assets/477e13ba-ad2c-4b1f-91c6-fa1e68d0659f



Para utilizarlo, el ESP32 y el equipo que abre la página deben estar conectados a la misma red Wi-Fi. Después de cargar el programa, se obtiene la IP asignada (ejemplo: `172.20.24.186`)[cite: 9] en el monitor serial y se ingresa esa dirección en el navegador web.

Este es el programa que se confirmó como funcional durante la práctica del taller. Para esta entrega se sustituyeron las credenciales de red por marcadores de posición y se configuró el texto de la página para indicar el control del LED externo.

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
## Conclusiones

- **Procesamiento de Señales Analógicas:** La integración de un promedio móvil de lecturas consecutivas en el ADC del ESP32 demostró ser una técnica efectiva para mitigar el ruido eléctrico y estabilizar las mediciones tanto del potenciómetro como del sensor LDR, permitiendo una conversión adecuada a voltaje y porcentaje relativo.
- **Conectividad y Escaneo de Redes:** A través de la librería `WiFi.h`, se comprobó la capacidad del ESP32 para analizar el entorno de radiofrecuencia de manera eficiente, identificando la intensidad de señal (RSSI) y los protocolos de seguridad de las redes Wi-Fi cercanas antes de establecer una conexión.
- **Integración IoT en la Nube:** La transmisión de datos hacia ThingSpeak confirmó el potencial del ESP32 para actuar como un nodo de telemetría remoto, permitiendo registrar, graficar y monitorear variables físicas en tiempo real desde cualquier plataforma con acceso a internet.
- **Control Web Embebido:** La implementación del servidor web interno en el ESP32 demostró la factibilidad de desplegar interfaces HTTP livianas para la interacción hombre-máquina (HMI), permitiendo el control dinámico de actuadores (como el LED externo) a través de solicitudes HTTP GET desde navegadores en la red local.

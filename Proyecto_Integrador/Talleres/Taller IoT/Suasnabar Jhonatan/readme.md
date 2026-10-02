# Informe práctico: Aplicaciones IoT utilizando ESP32

En esta práctica se exploraron distintas funciones de un sistema IoT basado en ESP32. Se realizaron lecturas analógicas con un potenciómetro y un LDR, se revisó la detección y conexión a redes WiFi, se enviaron datos a ThingSpeak y se implementó el control de un LED mediante una interfaz web.

## Materiales utilizados

- Placa ESP32, cable USB, protoboard y jumpers.
- Potenciómetro, fotorresistencia (LDR), LED y resistencias adecuadas para los montajes.
- Entorno Arduino IDE, acceso a una red WiFi y un canal configurado en ThingSpeak.
- Bibliotecas utilizadas según el ejercicio: `WiFi.h`, `WebServer.h` y `ThingSpeak.h`.

## Actividad 01 — Medición analógica con potenciómetro

Se conectó el potenciómetro al pin analógico GPIO 34 del ESP32. La primera prueba permitió visualizar las variaciones de la señal en el monitor serial, con una actualización cada medio segundo.



<img width="1280" height="960" alt="03_potenciometro_nube" src="https://github.com/user-attachments/assets/94fd2cef-6c87-4d2b-9654-8a7882d3cca7" />


La imagen muestra la prueba de lectura directa. Además, se presenta una variante del programa que calcula el promedio de varias muestras y estima el voltaje, funcionalidad que también se emplea en el envío de información a ThingSpeak.

### Conexión de referencia

| Terminal del potenciómetro | ESP32 |
|---|---|
| Extremo | 3.3 V |
| Otro extremo | GND |
| Central | GPIO 34 |

En la versión complementaria se toman diez muestras consecutivas y se obtiene su media aritmética. A partir de ese promedio se calcula una estimación de voltaje mediante `voltaje = promedio × 3.3 / 4095`. El cálculo sirve como referencia y no sustituye una calibración del convertidor analógico-digital.


### Código
<details>
<summary>Ver código</summary>
  
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
</details>

## Actividad 02 — Búsqueda y conexión a redes WiFi

En esta parte se ejecutó un escaneo de redes inalámbricas. El monitor serial presenta las redes encontradas junto con la intensidad de señal recibida; para ello se utiliza la función `WiFi.scanNetworks()`.


<img width="960" height="1280" alt="02_escaneo_wifi" src="https://github.com/user-attachments/assets/72cd5cd6-9b7b-4071-94a6-7e4ed34a2f79" />


También se incorpora un ejemplo de conexión a una red determinada. Cuando la conexión se establece, el ESP32 informa su dirección IP mediante `WiFi.localIP()`. La imagen adjunta corresponde únicamente al escaneo de redes y no a la visualización de la dirección IP.


### Código
<details>
<summary>Ver código</summary>

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
  
</details>


## Actividad 03 — Envío de datos del potenciómetro a ThingSpeak

Se configuró el potenciómetro en el GPIO 34 para registrar el promedio de las lecturas ADC y el voltaje aproximado. Ambos valores se publican en un canal de ThingSpeak después de procesar diez muestras.

| Campo | Dato |
|---|---|
| Field 1 | ADC promedio |
| Field 2 | Voltaje estimado, en V |


<img width="960" height="1280" alt="01_potenciometro" src="https://github.com/user-attachments/assets/2f9631f3-e7b9-45b9-a115-40e1b7672fb9" />


En las capturas del monitor serial se observan confirmaciones de envío y valores que llegan a 4095. Según la conversión aplicada, ese valor corresponde a 3.300 V. El programa espera quince segundos entre publicaciones, sin contar el tiempo que requieren la adquisición y la comunicación.


### Código
<details>
<summary>Ver código</summary>

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

</details>


## Actividad 04 — Registro de iluminación mediante un LDR

En el montaje se incorporó una fotorresistencia LDR. El programa adquiere la señal por el GPIO 34, calcula la media de diez lecturas y expresa el resultado como porcentaje relativo utilizando `valorLDR / 4095 × 100`.

| Campo | Dato |
|---|---|
| Field 1 | Lectura promedio del LDR |
| Field 2 | Porcentaje relativo de la lectura ADC |


<img width="1280" height="960" alt="04_ldr_nube" src="https://github.com/user-attachments/assets/e27b4824-b6ab-4aed-bf4b-f92067e4446f" />


Los registros permiten apreciar cambios en la señal durante la experimentación. Entre los valores que aparecen en las capturas se encuentran 3428.30, equivalente a 83.7 %, y posteriormente cifras próximas a 1062.30, equivalentes a 25.9 %.

Es importante considerar que el porcentaje calculado únicamente normaliza la lectura del ADC y **no equivale a una medición de iluminancia en lux**. La respuesta del circuito ante el aumento de luz depende de cómo estén conectados el LDR y la resistencia del divisor de tensión. Como las imágenes no permiten identificar con certeza el valor de dicha resistencia, no se especifica uno.


### Código
<details>
<summary>Ver código</summary>

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
</details>


## Actividad 05 — Accionamiento de un LED mediante una página web

El ESP32 se configuró como servidor web para manejar un LED externo conectado al GPIO 23. La interfaz incluye dos botones, uno para encender y otro para apagar el dispositivo. Al seleccionarlos, el navegador solicita las rutas `/encender` o `/apagar`, y el microcontrolador actualiza el estado de su salida digital.

### Evidencia


<img width="1280" height="960" alt="05_led_web" src="https://github.com/user-attachments/assets/d5e8a26f-21e2-42cd-87d5-1073e9e1d892" />


https://github.com/user-attachments/assets/349d789e-e085-4fb3-a8db-aa03713f4059



Para realizar la prueba, tanto el ESP32 como el dispositivo desde el que se accede al sitio deben encontrarse en la misma red local. Una vez cargado el programa, se identifica la IP mostrada en el monitor serial y se introduce esa dirección en el navegador.

El código incluido implementa el servidor y las acciones de control. Antes de ejecutarlo, deben configurarse las credenciales de la red y mantenerse la descripción del montaje acorde con el uso de un LED externo.


### Código
<details>
<summary>Ver código</summary>


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

## Pasos para ejecutar los programas

1. Crear un sketch en Arduino IDE y pegar el código correspondiente a la actividad.
2. Elegir el modelo de placa ESP32 y el puerto al que está conectado.
3. Verificar que esté instalado el paquete de placas ESP32 y añadir la biblioteca ThingSpeak para las actividades 03 y 04.
4. Introducir el nombre y la contraseña de la red en los campos `TU_WIFI` y `TU_PASSWORD`.
5. En los ejercicios con ThingSpeak, colocar el identificador real del canal en `channelID` y la clave de escritura en `TU_WRITE_API_KEY`. Revisar también la asignación de los campos indicada en cada tabla.
6. Cargar el sketch y abrir el monitor serial con una velocidad de **115200 baudios**.






Los datos de acceso incluidos son valores de ejemplo y deben reemplazarse por los correspondientes a la red utilizada. Los programas esperan hasta conseguir conexión, por lo que unas credenciales incorrectas impedirán que continúen con su ejecución normal.


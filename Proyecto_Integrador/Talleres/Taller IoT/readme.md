# Proyecto Node-RED 

En esta actividad se implementó un sistema de monitoreo utilizando un **ESP32**, un sensor **DHT11**, comunicación mediante **MQTT** y una interfaz gráfica desarrollada en **Node-RED Dashboard**.

Además, se agregó un control mediante MQTT para encender y apagar un LED conectado al ESP32 desde el Dashboard.

---

## 1. Materiales utilizados

Para realizar la actividad se utilizaron los siguientes componentes:

- ESP32
- Sensor DHT11
- Protoboard
- Cables jumper
- Computadora con Arduino IDE
- Node-RED
- Node-RED Dashboard 2.0
- Broker MQTT proporcionado para el taller

---

## 2. Conexión del sensor DHT11

El sensor DHT11 se conectó al ESP32 utilizando el GPIO 4 para la lectura de datos.

La conexión utilizada fue:

| DHT11 | ESP32  |
|-------|--------|
| VCC   | 3.3V   |
| DATA  | GPIO 4 |
| GND   | GND    |

Para el control del LED se utilizó el GPIO 2 del ESP32.

---

## 3. Configuración del broker MQTT

Para comunicar el ESP32 con Node-RED se utilizó el broker MQTT proporcionado para el taller.

Los datos utilizados fueron:

```
HOST: mqtt.rcr-labs.com
PORT: 1883
USER: alumno
PASSWORD: UPCH2026
```

Como nuestro grupo corresponde al **Equipo 8**, se utilizaron los siguientes tópicos:

```
equipo08/sensor/datos
equipo08/actuadores/led
```

El primer tópico se utiliza para publicar los valores obtenidos por el sensor DHT11.

```
equip0o8/sensor/datos
```

El segundo tópico se utiliza para controlar el LED del ESP32 desde Node-RED.

```
equipo08/actuadores/led
```

---

## 4. Programación del ESP32

En el ESP32 se utilizaron las librerías necesarias para realizar la conexión WiFi, comunicación MQTT, creación del mensaje JSON y lectura del sensor DHT11.

Las principales librerías utilizadas fueron:

```cpp
#include <WiFi.h>
#include <PubSubClient.h>
#include <ArduinoJson.h>
#include <DHT.h>
```

El código completo utilizado fue el siguiente:

```cpp
#include <WiFi.h>
#include <PubSubClient.h>
#include <ArduinoJson.h>

// ================= CONFIGURACIÓN WIFI =================
const char* WIFI_SSID = "hyu";
const char* WIFI_PASS = "123456789";

// ================= CONFIGURACIÓN MQTT =================
// Puedes usar el dominio o la IP directa 108.181.195.81
const char* MQTT_SERVER   = "mqtt.rcr-labs.com"; 
const int   MQTT_PORT     = 1883;

// Credenciales configuradas en EMQX (Autenticación interna)
const char* MQTT_USER     = "alumno";          // o equipo0, equipo1, etc.
const char* MQTT_PASSWORD = "UPCH2026";
const char* CLIENT_ID     = "ESP32_Equipo08";

// Topics MQTT
const char* TOPIC_PUB     = "equipo08/sensor/datos"; 
const char* TOPIC_SUB     = "equipo08/actuadores/led";

// ================= OBJETOS Y VARIABLES ===============
WiFiClient espClient;
PubSubClient client(espClient);

unsigned long ultimoEnvio = 0;
const long intervaloEnvio = 5000; // Envío cada 5 segundos (no bloqueante)

// Conexión a la red Wi-Fi
void setupWiFi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a Wi-Fi: ");
  Serial.println(WIFI_SSID);

  WiFi.mode(WIFI_STA);
  WiFi.begin(WIFI_SSID, WIFI_PASS);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nWiFi conectado con éxito");
  Serial.print("Dirección IP local: ");
  Serial.println(WiFi.localIP());
}

// Recepción de mensajes suscritos (por si deseas controlar actuadores desde Node-RED)
void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido en topic [");
  Serial.print(topic);
  Serial.print("]: ");

  String mensaje = "";
  for (unsigned int i = 0; i < length; i++) {
    mensaje += (char)payload[i];
  }
  Serial.println(mensaje);

  // Ejemplo: procesar comando
  if (String(topic) == TOPIC_SUB) {
    if (mensaje == "ON") {
      digitalWrite(2, HIGH);
      Serial.println("Comando: Encender LED");
    } else if (mensaje == "OFF") {
      digitalWrite(2, LOW);
      Serial.println("Comando: Apagar LED");
    }
  }
}

// Reconexión automática al broker EMQX
void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conectar con broker MQTT...");
    
    // Autenticación con credenciales en EMQX
    if (client.connect(CLIENT_ID, MQTT_USER, MQTT_PASSWORD)) {
      Serial.println(" ¡Conectado!");
      
      // Suscribirse a tópicos de control si es necesario
      client.subscribe(TOPIC_SUB);
      Serial.print("Suscrito a: ");
      Serial.println(TOPIC_SUB);
    } else {
      Serial.print(" Falló. Código de error rc=");
      Serial.print(client.state());
      Serial.println(" Reintentando en 5 segundos...");
      delay(5000);
    }
  }
}

void setup() {
  Serial.begin(115200);
  setupWiFi();

  client.setServer(MQTT_SERVER, MQTT_PORT);
  client.setCallback(callback);
  pinMode(2,OUTPUT);
}

void loop() {
  // Asegurar persistencia de la sesión MQTT
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  // Envío periódico sin usar delay() para no congelar la recepción
  unsigned long ahora = millis();
  if (ahora - ultimoEnvio >= intervaloEnvio) {
    ultimoEnvio = ahora;

    // Simulación de lectura de sensores (ej. DHT22 o BMP280)
    float tempSimulada = 24.0 + (random(0, 100) / 10.0);
    float humSimulada  = 55.0 + (random(0, 200) / 10.0);

    // Creación del documento JSON
    StaticJsonDocument<200> doc;
    doc["dispositivo"] = CLIENT_ID;
    doc["temperatura"] = serialized(String(tempSimulada, 2));
    doc["humedad"]     = serialized(String(humSimulada, 2));

    char jsonBuffer[256];
    serializeJson(doc, jsonBuffer);

    // Publicación hacia EMQX
    Serial.print("Publicando en ");
    Serial.print(TOPIC_PUB);
    Serial.print(": ");
    Serial.println(jsonBuffer);

    client.publish(TOPIC_PUB, jsonBuffer);
  }
}
```
---

## 5. Envío de información mediante JSON

El ESP32 obtiene los valores de temperatura y humedad del sensor DHT11 y genera un mensaje en formato JSON.

Un ejemplo del mensaje enviado es:

```json
{
  "dispositivo": "ESP32_Equipo8",
  "temperatura": 24.4,
  "humedad": 60.2
}
```

Este mensaje es publicado cada 3 segundos en el tópico:

```
equipo8/sensor/datos
```

---

# Configuración de Node-RED

## 6. Nodo MQTT de entrada

En Node-RED se agregó un nodo **MQTT IN** para recibir los mensajes enviados por el ESP32.

Se configuró el broker con los siguientes datos:

```
Server: mqtt.rcr-labs.com
Port: 1883
Username: alumno
Password: UPCH2026
```

El tópico utilizado fue:

```
equipo8/sensor/datos
```

Cuando la conexión con el broker se realizó correctamente, debajo del nodo MQTT apareció el mensaje:

```
connected
```

Esto confirmó que Node-RED ya podía recibir la información publicada por el ESP32.

---

## 7. Separación de los datos recibidos

Después del nodo MQTT se utilizaron tres nodos tipo **change**.

Estos permitieron separar las variables recibidas dentro del JSON.

### Temperatura

En el primer nodo se obtuvo el valor:

```
msg.payload.temperatura
```

y se guardó nuevamente en:

```
msg.payload
```

Este dato se envió al indicador y al gráfico de temperatura.

---

### Humedad

En el segundo nodo se obtuvo:

```
msg.payload.humedad
```

Este valor se utilizó para el indicador y el gráfico de humedad.

---

### Dispositivo

En el tercer nodo se obtuvo:

```
msg.payload.dispositivo
```

Este dato se mostró en el Dashboard para identificar el dispositivo que estaba enviando la información.

---

## 8. Indicador de temperatura

Para mostrar la temperatura se agregó un nodo **Gauge** del Dashboard.

Este recibe el valor:

```
msg.payload
```

El indicador permite observar de forma visual la temperatura medida por el DHT11.

También se agregó un gráfico para poder observar cómo cambia la temperatura con el tiempo.

---

## 9. Indicador de humedad

Se agregó otro nodo **Gauge** para mostrar la humedad relativa.

El valor utilizado también fue:

```
msg.payload
```

Además, se agregó un gráfico para observar la evolución de la humedad.

---

## 10. Identificación del dispositivo

Se agregó un nodo de texto para mostrar el dispositivo conectado.

El ESP32 envía el siguiente identificador:

```
ESP32_Equipo8
```

De esta manera es posible identificar desde qué equipo se están recibiendo los datos.

---

## 11. Control del LED

También se agregó un **Switch** dentro del Dashboard para controlar el LED conectado al GPIO 2 del ESP32.

El switch envía dos posibles mensajes:

```
ON
```

para encender el LED y:

```
OFF
```

para apagarlo.

Estos mensajes son enviados mediante un nodo **MQTT OUT** al tópico:

```
equipo8/actuadores/led
```

El ESP32 se encuentra suscrito a este tópico y ejecuta la acción dependiendo del mensaje recibido.

---

## 12. Flujo final en Node-RED

Finalmente, el flujo quedó formado por:

- Un nodo MQTT para recibir los datos.
- Tres nodos para separar temperatura, humedad y dispositivo.
- Dos indicadores Gauge.
- Dos gráficos.
- Un indicador de texto.
- Un Switch para controlar el LED.
- Un nodo MQTT de salida.

El flujo final desarrollado en Node-RED se muestra a continuación:

<img width="852" height="422" alt="rama" src="https://github.com/user-attachments/assets/bcca278a-2365-40c1-aee8-bcadb34ae833" />


En la imagen se observa que el nodo **MQTT IN**, suscrito al tópico:

```
equipo8/sensor/datos
```

se encuentra conectado correctamente al broker, ya que muestra el estado `connected`.

Los datos recibidos pasan por tres nodos **change** (`set msg.payload`), que los separan en:

```
Temperatura
Humedad
Dispositivo
```

Posteriormente, cada dato es enviado al elemento correspondiente del Dashboard:

- **Temperatura:** al gauge `Temperatura` y al gráfico `Evolucion temperatura`.
- **Humedad:** al gauge `Humedad` y al gráfico `Evolucion Humedad`.
- **Dispositivo:** al nodo de texto `Dispositivo`.

En la parte inferior se encuentra el nodo **Switch LED**, que está conectado al nodo MQTT de salida `Comandos Publicados`, encargado de publicar los comandos hacia el ESP32. En la captura, el switch aparece en estado `on` y el nodo MQTT de salida también figura como `connected`.

---

# Dashboard

## 13. Visualización de resultados

Después de realizar la configuración de los nodos y establecer la conexión con el dispositivo ESP32, se ingresó al Dashboard de Node-RED para visualizar en tiempo real los datos obtenidos por el sensor DHT11.

<img width="1442" height="647" alt="Dashboard" src="https://github.com/user-attachments/assets/1a05c146-d132-4891-8469-d68e206baea0" />

Durante la prueba mostrada en la imagen se registraron aproximadamente los siguientes valores:

Temperatura: 24.4 °C  
Humedad: 60 %  
Dispositivo: ESP32_Equipo8

---

## 14. Resultado

Se logró establecer correctamente la comunicación entre los diferentes componentes del sistema:

```text
ESP32 <-> MQTT <-> Node-RED





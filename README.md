# PR3_EricAyala

## PRACTICA 3: WIFI y BLUETOOTH 📶

### Practica A generación de una pagina web

**CODIGO:**
```CPP
#include <WiFi.h>
#include <WebServer.h>
// SSID & Password


const char* ssid = "iphone de AyalaKT"; // Enter your SSID here
const char* password = "12345678"; //Enter your Password here


WebServer server(80); // Object of WebServer(HTTP port, 80 is defult)


void setup() {
Serial.begin(115200);
Serial.println("Try Connecting to ");
Serial.println(ssid);


// Connect to your wi-fi modem
WiFi.begin(ssid, password);


// Check wi-fi is connected to wi-fi network
while (WiFi.status() != WL_CONNECTED) {
delay(1000);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected successfully");
Serial.print("Got IP: ");
Serial.println(WiFi.localIP()); //Show ESP32 IP on serial
server.on("/", handle_root);
server.begin();
Serial.println("HTTP server started");
delay(100);
}
void loop() {
server.handleClient();
}


// HTML & CSS contents which display on web server
String HTML = "<!DOCTYPE html>\
<html>\
<body>\
<h1>My Primera Pagina con ESP32 - Station Mode &#128522;</h1>\
</body>\
</html>";


// Handle root url (/)
void handle_root() {
server.send(200, "text/html", HTML);
}

```
### 1. Informe del funcionamiento, Salida por el Terminal y Visualización en Navegador de la Conexión a la Página Web

**FUNCIONAMIENTO**

El objetivo de este código es establecer un servidor web en un ESP32, en este caso, con mi móvil. Luego esta mostrará una página HTML cuando se acceda, a través de un navegador web.

- Primeramente la ESP32 se conecta a la red WiFi que le damos en el código.

  ```cpp
  const char* ssid = "iphone de AyalaKT"; // Enter your SSID here
  const char* password = "12345678"; //Enter your Password here
  ```
- Creamos un objeto *WebServer* y se define una ruta. Esta la manejamos con la función `handle_root()`.

- La funcion **handle_root()** se llama cuando se accede al servidor web.Esta envía una respuesta la cual en este caso será la web.  

  **SALIDA POR EL TERMINAL**
  
  Por el terminal podremos ver información sobre el WiFi y la dirección IP asignada al ESP32. Si se conecta correctamente a la red WiFi, mostrará un mensaje indicando la conexión exitosa y la dirección IP del ESP32.

  ```cpp
    while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
   }
    Serial.println("");
    Serial.println("WiFi connected successfully");
    Serial.print("Got IP: ");
    Serial.println(WiFi.localIP()); //Show ESP32 IP on serial
    server.on("/", handle_root);
      server.begin();
    Serial.println("HTTP server started");
    delay(100);

  ```
  ### Practica B comunicación bluetooth con el movil

  **CODIGO:**
```cpp

#include "BluetoothSerial.h"
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif
BluetoothSerial SerialBT;

void setup() {
Serial.begin(115200);
SerialBT.begin("ESP32test"); //Bluetooth device name
Serial.println("The device started, now you can pair it with bluetooth!");
}

void loop() {
if (Serial.available()) {
SerialBT.write(Serial.read());
}
if (SerialBT.available()) {
Serial.write(SerialBT.read());
}
delay(20);
}
```
**FUNCIONAMIENTO**

- Para la configuración Bluetooth utilizamos la biblioteca `BluetoothSerial.h` y se inicia la comunicación con un nombre de dispositivo específico utilizando `SerialBT.begin("ESP32test")`.

- En el bucle principal `loop()`, ,si hay datos disponibles en la conexión serial, se leen y se envían a través de la conexión Bluetooth `(SerialBT.write(Serial.read()))`. Del mismo modo, si hay datos disponibles en la conexión Bluetooth, se leen y se envían a través de la conexión serial `(Serial.write(SerialBT.read()))`.

```cpp
void loop() {
if (Serial.available()) {
SerialBT.write(Serial.read());
}
if (SerialBT.available()) {
Serial.write(SerialBT.read());
}
delay(20);
}
```

- En la salida se imprime un mensaje indicando que el dispositivo se ha iniciado correctamente y está listo para la conexión Bluetooth.

De esta manera, hemos establecido una conexión Bluetooth con nuestros móviles, utilizando la aplicación *Serial Bluetooth Terminal*, la cual nos permitía poder intercambiar mensajes entre los dos dispositivos, el móvil y la ESP32 

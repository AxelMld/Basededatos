# **PRÁCTICA 10 BASE DE DATOS**
--------------------------------------
## **INSTALACION DE XAMPP** ## 

1. Nos dirigimos al siguiente enlace 

https://www.apachefriends.org/es/index.html

2. Damos Clic en descargar y aceptamos termino y condiciones una vez instalado nos aparece la siguiente ventana 

![](https://github.com/AxelMld/Basededatos/blob/main/xampp.jpeg?raw=true)

3. Procedemos a encender los botones appache y MySQL que aparecen en la ventana mostrada anteriormente 


4. Damos clic en admin y nos dirigue a PHPmyadmin 

![](https://github.com/AxelMld/Basededatos/blob/main/imagen%20de%20db.jpeg?raw=true)

5. Damos clic en la parte izquierda para agregar una base de datos, le ponemos el nombre deseado y procedemos a agregar las variables con su tipo de dato. 


6. Creamos la base de datos y realizamos la siguiente configuracion en la base de datos creada como se muestra en la imagen, respetamos el tipo de dato mostrado 

![](https://github.com/AxelMld/Basededatos/blob/main/imagen%20de%20db.jpeg?raw=true)

7. Una vez realizado nos dirigimos a nodeJS

---------------------------------------

## **INICIO DE NODEJS** ##

1. Se abre el programa "CMD" en modo administrador.
2. Una vez abierto se ponen el siguiente comando.

                    2. Para arrancar nodejs se usa el comando **node- red** 

Aparecera lo siguiente ya inicio el nodejs, se deja abierto para su uso 

![](https://github.com/AxelMld/Practica6-enviodedatos/blob/main/arranque%20nodejs.png?raw=true)

3. Nos Dirigimos al navegador buscamos la pagina **localhost:1880**

Aparecera la siguiente imagen lugar de trabajo de nodejs


![](https://github.com/AxelMld/Practica6-enviodedatos/blob/main/nodejs%20trabajo.png?raw=true)

----------------------------------------------------------------

## **REALIZACION Y CONFIGURACION DE ESTRUCTURA EN NODEJS** ##

1. Se realiza la siguiente estructura 

![](https://github.com/AxelMld/Basededatos/blob/main/estructura.jpeg?raw=true)




## CONFIGURACION ESTRUCTURA NODEJS

1. En apartado de Base de datos se tiene que realiza lo siguiente 

* ir a la seccion de privilegios en phpmyadmin y copiar el nombre del servidor lo encontraras en la siguiente imagen 

![](https://github.com/AxelMld/Basededatos/blob/main/phproot.jpeg?raw=true)

* La direccion ip encontrada en phpmyadmin se copia en la seccion de base de datos de nodered como se muestra a continuacion 

![](https://github.com/AxelMld/Basededatos/blob/main/base%20de%20datos.jpeg?raw=true) 



2. direccion IP 44.195.202.69 para **mqtt in**




3. Configuracion de la funcion de base de datos se agrego el siguiente comando 


```
var query = "INSERT INTO `trabajo 1`(`ID`, `FECHA`, `DEVICE`, `TEMPERATURA`, `HUMEDAD`, `DISTANCIA`) VALUES (NULL, current_timestamp(), '";
query = query + msg.payload.DEVICE + "','";
query = query + msg.payload.TEMPERATURA + "','";
query = query + msg.payload.HUMEDAD + "','";
query = query + msg.payload.DISTANCIA + "');'";
msg.topic = query;
return msg;

```


4. Se realizan las siguientes configuraciones en la funcion de node red 


* Configuracion para Temperatura 


```
msg.payload = msg.payload.Temperatura;
msg.topic = "Temperatura";
return msg;

```

* Configuracion para Humedad


```
msg.payload = msg.payload.Humedad;
msg.topic = "Humedad";
return msg;

```

* Configuracion para Distancia 


```
msg.payload = msg.payload.Distancia;
msg.topic = "Distancia";
return msg;


```
---------------------------------------------------------------

# **CONEXION Y CODIGO EN WOKWI**

## Para la simulación nos dirigimos a la página  

woki : https://wokwi.com

#### Dashboard de página a utilizar 
![](https://github.com/AxelMld/Practica-3-Sensor-Ultrasonico-/blob/main/dash.png?raw=true)


## Para está práctica se utilizó 

* Módulo ESP32 
* Sendor DHT22
* Sensor Ultrasonic Distance 
* Relay


## Pasos a Seguir 

1. Seleccionar el módulo ESP32.
2. Escoger los dispositivos a utilizar anteriormente mensionados. 
3. Realizar las conexiones como se muestra en el apartado "conexión".
4. Agregar las librerias mensiadas en el apartado Librerías 




## **Conexión**

![](https://github.com/AxelMld/Practica8-DisTemyHum/raw/main/conexion.png?raw=true)


## **Librerías**

1. ArduinoJson
2. PubSubClient
3. SimpleWiFiClient

## Código utilizado 


```


#include <ArduinoJson.h>
#include <WiFi.h>
#include <PubSubClient.h>
#include "DHTesp.h"

#define BUILTIN_LED 2
const int Trigger = 13;   //Pin digital 2 para el Trigger del sensor
const int Echo = 12;   //Pin digital 3 para el Echo del sensor



//pines y seccion del sensor DHT22
const int DHT_PIN = 15;

DHTesp dhtSensor;


// Update these with values suitable for your network.

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqtt_server = "44.195.202.69";
String username_mqtt="AXELM";
String password_mqtt="1234";

WiFiClient espClient;
PubSubClient client(espClient);
unsigned long lastMsg = 0;
#define MSG_BUFFER_SIZE  (50)
char msg[MSG_BUFFER_SIZE];
int value = 0;

void setup_wifi() {

  delay(10);
  // We start by connecting to a WiFi network
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  randomSeed(micros());

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  // Switch on the LED if an 1 was received as first character
  if ((char)payload[0] == '1') {
    digitalWrite(BUILTIN_LED, LOW);   
    // Turn the LED on (Note that LOW is the voltage level
    // but actually the LED is on; this is because
    // it is active low on the ESP-01)
  } else {
    digitalWrite(BUILTIN_LED, HIGH);  
    // Turn the LED off by making the voltage HIGH
  }

}

void reconnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    // Create a random client ID
    String clientId = "ESP8266Client-";
    clientId += String(random(0xffff), HEX);
    // Attempt to connect
    if (client.connect(clientId.c_str(), username_mqtt.c_str() , password_mqtt.c_str())) {
      Serial.println("connected");
      // Once connected, publish an announcement...
      client.publish("outTopic", "hello world");
      // ... and resubscribe
      client.subscribe("inTopic");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

void setup() {
  pinMode(BUILTIN_LED, OUTPUT);     // Initialize the BUILTIN_LED pin as an output
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);

  Serial.begin(9600);//iniciailzamos la comunicación
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
 
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0


}

void loop() {

long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

 TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(1000);          //Hacemos una pausa de 100ms

  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  unsigned long now = millis();
  if (now - lastMsg > 2000) {
    lastMsg = now;
    //++value;
    //snprintf (msg, MSG_BUFFER_SIZE, "hello world #%ld", value);

    StaticJsonDocument<128> doc;

    doc["DEVICE"] = "ESP32";
    //doc["Anho"] = 2022;
    //doc["Empresa"] = "Educatronicos";
    doc["Distancia"] = String(d);
    doc["Temperatura"] = String(data.temperature, 1);
    doc["Humedad"] = String(data.humidity, 1);
    
   

    String output;
    
    serializeJson(doc, output);

    Serial.print("Publish message: ");
    Serial.println(output);
    Serial.println(output.c_str());
    client.publish("DAIyM/AxelM/P8", output.c_str());
  }
}


```
-------------------------

# **RESULTADOS**

## 1. Resultado en la dashboard 

![](https://github.com/AxelMld/Basededatos/blob/main/resultado%20dash.jpeg?raw=true)




## 2. Resultado en la en la base de datos arrojado  


![](https://github.com/AxelMld/Basededatos/blob/main/resultado.jpeg?raw=true)



# Creditos 

**Axel Miranda.**

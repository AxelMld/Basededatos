# **PRÁCTICA 10 BASE DE DATOS **

## ** INSTALACION DE XAMPP ** ## 

1. Nos dirigimos al siguiente enlace 

https://www.apachefriends.org/es/index.html

2. Damos Clic en descargar y aceptamos termino y condiciones una vez instalado nos aparece la siguiente ventana 

![]()

3. Procedemos a encender los botones appache y MySQL que aparecen en la ventana mostrada anteriormente 

4. Damos clic en admin y nos dirigue a PHPmyadmin 

1[]()

5. Creamos la base de datos y realizamos la siguiente configuracion en la base de datos creada como se muestra en la imagen, respetamos el tipo de dato mostrado 

6. Una vez realizado nos dirigimos a nodeJS


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

![]()

2. En la seccion base de datos en node-red se realiza la siguiente configuracion conectandola a la base de datos creada en xampp y phpmyadmin 



### Configuracion y herramientas usadas 

* Para axelm se utilizo  **mqtt out** "nombre utilizado 

--------------------------------------------------------------

## CONFIGURACION ESTRUCTURA NODEJS

* Para la configuracion de mqtt in se utilizo la siguiente 

![]()

1. direccion IP 44.195.202.69 

2. Configuracion de la funcion de base de datos se agrego el siguiente comando 


3. Se realizan las siguientes configuraciones en la funcion de node red 


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


```


# **RESULTADO**

## 1. Resultado en la dashboard 

![]()




## 2. Resultado en la en la base de datos arrojado  


![]()



# Creditos 

**Axel Miranda.**

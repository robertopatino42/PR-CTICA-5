# PR-CTICA-5

Este repositorio muestra como podemos programar una ESP32 con el sensor Ultrasonico y una pantalla LCD y el sensor DHT.

INTRODUCCIÓN

MATERIAL

Para realizar esta practica necesitas lo siguiente

WOKWI
Tarjeta ESP 32
Sensor DHT11
HC-SR04 Ultrasonic Distance Sensor
LCD 16x2 (I2C)

Instrucciones de preparación de entorno

1- Entrar a la plataforma de Wokwi e introducir el sigueinte código:

#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

DHTesp dhtSensor;

const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 2;   //Pin digital 3 para el Echo del sensor
const int DHT_PIN = 15;



LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
  Serial.begin(9600);//iniciailzamos la comunicación
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
   Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();
}

void loop()
{

  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---"); 

  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");

  delay(2000);
  lcd.clear();

  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm

  lcd.setCursor(0, 0);
  lcd.print("Distancia: " +String(d) + "cm  ");
  lcd.print("Wokwi Online IoT");
  delay(2000);
  lcd.clear();

  
  

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("RMPL ");
  lcd.setCursor(0, 1);
  lcd.print("ING MECÁNICO");
  delay(2000);
  lcd.clear();

}

2- Instalar la libreria DHT sensor library for ESPx y LiquidCrystal I2C como se muestra en la siguente imagen.

![Captura de pantalla 2024-01-23 232129](https://github.com/robertopatino42/PR-CTICA-5/assets/153964688/2c39d964-3c21-443c-aebd-151bcdfb7827)

3- Hacer la conexion de los sensores Ultrasonico y DHT11 con la ESP32 y la Lcd como se muestra en la siguente imagen.

![Captura de pantalla 2024-01-23 232614](https://github.com/robertopatino42/PR-CTICA-5/assets/153964688/d7b3bbc9-65b2-4464-bacc-2a8b756f3d89)

RESULTADO

Cuando haya funcionado, verás los valores dentro del monitor serial como se muestra en la siguente imagen.

![Captura de pantalla 2024-01-23 233015](https://github.com/robertopatino42/PR-CTICA-5/assets/153964688/3869f019-9001-489f-96f3-aeeb92cfd14f)


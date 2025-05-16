#include <WiFi.h>
#include "ThingSpeak.h"
#include <LiquidCrystal_I2C.h>
#include <Wire.h>
#include <SPI.h>
const char* ssid = "project";   // your network SSID (name) 
const char* password = "project1234";   // your network password
WiFiClient  client;
unsigned long myChannelNumber = 2924022;
const char * myWriteAPIKey = "RRWDFP02GZGM01GV";
int load1 = 1,load2 = 2,load3=3,theft=4;
unsigned long lastTime = 0;
unsigned long timerDelay = 30000;
LiquidCrystal_I2C lcd(0x27, 16, 2);
const int bz = 19;
const int relay = 23;
const int flow = 39;
const int waterlevel = 36;
long currentMillis = 0;
long previousMillis = 0;
int interval = 1000;
booleanledState = LOW;
float calibrationFactor = 4.5;
volatile byte pulseCount;
byte pulse1Sec = 0;
float flowRate;
int flowMilliLitres;
int totalMilliLitres;
int a = 1,b = 2, c = 3,lk = 0,wl = 0,ms=0;

void IRAM_ATTR pulseCounter()
{
pulseCount++;
}
void setup() {
Serial.begin(9600);  //Initialize serial
pinMode(flow,INPUT_PULLUP);
pinMode(waterlevel,INPUT);
pinMode(bz, OUTPUT);
pinMode(relay, OUTPUT);
digitalWrite(relay,LOW);
digitalWrite(bz,LOW);// MOTOR OFF
lcd.begin();         // initialize the lcd
lcd.backlight();    // open the backlight
digitalWrite(relay,HIGH);
welcome_note();
Serial.println("STATION MODE");
digitalWrite(relay,LOW);
WiFi.mode(WIFI_STA);   
ThingSpeak.begin(client);  // Initialize ThingSpeak
lcd.setCursor(0,0);
lcd.print("SSID:project");
lcd.setCursor(0,1);
lcd.print("PWD:project1234");
delay(3000);
digitalWrite(bz,LOW);
sensors_name();
pulseCount = 0;
flowRate = 0.0;
flowMilliLitres = 0;
totalMilliLitres = 0;
previousMillis = 0;
attachInterrupt(digitalPinToInterrupt(flow), pulseCounter, FALLING);
}
void loop() {
wl = analogRead(waterlevel);
Serial.print("WATER LEVEL = ");
Serial.println(wl);
if(wl< 100)
  {
digitalWrite(relay,HIGH);
ms = 1;
  }
  else
  {
digitalWrite(relay,LOW);
digitalWrite(bz,LOW);
ms = 0;
  }

currentMillis = millis();
  if (currentMillis - previousMillis> interval) {

    pulse1Sec = pulseCount;
pulseCount = 0;
flowRate = ((1000.0 / (millis() - previousMillis)) * pulse1Sec) / calibrationFactor;
previousMillis = millis();
flowMilliLitres = (flowRate / 60) * 1000;
totalMilliLitres += flowMilliLitres;
Serial.print("Flow rate: ");
Serial.print(int(flowRate));  // Print the integer part of the variable
Serial.print("L/min");
Serial.print("\t");       // Print tab space
Serial.print("Output Liquid Quantity: ");
Serial.print(totalMilliLitres);
Serial.print("mL / ");
Serial.print(totalMilliLitres / 1000);
Serial.println("L");
  } 
if(ms> 0)
  {
if(flowRate< 9)
    {
digitalWrite(bz,HIGH); 
lk = 1;
    }
    else
    {
digitalWrite(bz,LOW);
lk = 0;
    }
  }
if(lk> 0)
{
if(WiFi.status() != WL_CONNECTED){
Serial.print("Attempting to connect");
while(WiFi.status() != WL_CONNECTED){
WiFi.begin(ssid, password); 
delay(5000);     
      } 
Serial.println("\nConnected.");
    }
ThingSpeak.setField(a, flowRate);   // field  1
ThingSpeak.setField(b, totalMilliLitres);   //filed 2
ThingSpeak.setField(c, lk);   // field  3
    //int x = ThingSpeak.writeField(myChannelNumber, field4, temperatureC, myWriteAPIKey);
    int x = ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey);
if(x == 200){
Serial.println("Channel update successful.");
    }
else{
Serial.println("Problem updating channel. HTTP error code " + String(x));
    }
lastTime = millis();
}
}
void sensors_name()
{
lcd.clear();
lcd.setCursor(0,0);
lcd.print("WATER LEVEL");
lcd.setCursor(0,1);
lcd.print("FLOW:  ");
delay(1000);
  }
void welcome_note()
{
lcd.clear();
lcd.setCursor(0,0);
lcd.print("WATER FLOW LEAKAGE");
lcd.setCursor(0,1);
lcd.print("DETECTION");
delay(1000);
lcd.clear();
}

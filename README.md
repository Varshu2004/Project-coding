#define BLYNK_TEMPLATE_ID "TMPL3vL3T0oIC"
#define BLYNK_TEMPLATE_NAME "Water Bottle"
#define BLYNK_AUTH_TOKEN "GpC0SXIdjlnkfDHD0pm-1YRxPz6cuIUN"

// Your WiFi Credentials.
// Set password to "" for open networks.
char ssid[] = "phoenixiot1234";  // type your wifi name
char pass[] = "phoenixiot1234";  // type your wifi password

// define the GPIO connected with Sensors & LEDs

#define RAIN1_SENSOR   D6  //D1
#define RAIN2_SENSOR   D5
#define RAIN3_SENSOR   D4

#define WIFI_LED      16 //D0

//#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
 #include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27,16,2);

int RAIN1_SENSOR_Value = 0;
int RAIN2_SENSOR_Value = 0;
int RAIN3_SENSOR_Value = 0;
bool isconnected = false;
char auth[] ="GpC0SXIdjlnkfDHD0pm-1YRxPz6cuIUN";

 
#define VPIN_BUTTON_1    V1
#define VPIN_BUTTON_2    V2
#define VPIN_BUTTON_3    V3

BlynkTimer timer;

void checkBlynkStatus() { // called every 2 seconds by SimpleTimer
  getSensorData();
  isconnected = Blynk.connected();
  if (isconnected == true) {
    digitalWrite(WIFI_LED, LOW);
    sendSensorData();
    //Serial.println("Blynk Connected");
  }
  else{
    digitalWrite(WIFI_LED, HIGH);
    Serial.println("Blynk Not Connected");
  }
}

void getSensorData()
{
  
}

void sendSensorData()
{  
 
}

void setup()
{
  Serial.begin(9600);
 
  pinMode(RAIN1_SENSOR, INPUT);
pinMode(RAIN2_SENSOR, INPUT);
pinMode(RAIN3_SENSOR, INPUT);

  pinMode(WIFI_LED, OUTPUT);

  digitalWrite(WIFI_LED, HIGH);

  WiFi.begin(ssid, pass);
  timer.setInterval(2000L, checkBlynkStatus); // check if Blynk server is connected every 2 seconds
  Blynk.config(auth);
  delay(1000);
}

void loop()
{
   RAIN1_SENSOR_Value = digitalRead(RAIN1_SENSOR);
  RAIN2_SENSOR_Value = digitalRead(RAIN2_SENSOR);
  RAIN3_SENSOR_Value = digitalRead(RAIN3_SENSOR);
   if (RAIN1_SENSOR_Value == 0 ){
 Blynk.logEvent("Alert", "PLS Drinking Water");
    Blynk.virtualWrite(VPIN_BUTTON_1, "FULL");
    delay(4000);

 }
    if (RAIN2_SENSOR_Value == 0 ){
 Blynk.virtualWrite(VPIN_BUTTON_2, "MIDDLE");
    delay(4000);

 }
     if (RAIN3_SENSOR_Value == 0 ){
   Blynk.virtualWrite(VPIN_BUTTON_3, "LOW");
    delay(4000); 

 }
 else 
  {
    Blynk.virtualWrite(VPIN_BUTTON_1, "LEVEL DOWN");
    Blynk.virtualWrite(VPIN_BUTTON_2, "LEVEL DOWN");
    Blynk.virtualWrite(VPIN_BUTTON_3, "LEVEL DOWN");
  }
 
{
  Blynk.run();
  timer.run();
}
}# Project-coding

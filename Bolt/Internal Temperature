#include <ESP8266WiFi.h>
#include <Firebase_ESP_Client.h>
#include "addons/TokenHelper.h"
#include "addons/RTDBHelper.h"
#include "DHT.h"
#include <Adafruit_BMP085.h>

#define DHTPIN1 D4     // what pin we're connected to
#define DHTTYPE DHT11   // DHT 11 

Adafruit_BMP085 bmp;
DHT dht1(DHTPIN1, DHTTYPE);

#define WIFI_SSID "123456789"
#define WIFI_PASSWORD "123456789"
#define API_KEY "AIzaSyBHkLa5zlQxdYZXnt84XRo6TeiisA-ySFE"
#define DATABASE_URL "https://team-bolt-default-rtdb.firebaseio.com/"

FirebaseData fbdo;
FirebaseAuth auth;
FirebaseConfig config;
unsigned long sendDataPrevMillis = 0;
bool signupOK = false;
const int buzzer = D3;

void setup() {
  pinMode(buzzer, OUTPUT);
  Serial.begin(115200);
  Serial.println(); 
  Serial.println("DHTxx test!");
  dht1.begin();
  if (!bmp.begin()) {
  Serial.println("Could not find a valid BMP085 sensor, check wiring!");
  while (1) {}
  }
   WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  Serial.print("Connecting to Wi-Fi");
  while (WiFi.status() != WL_CONNECTED){
    Serial.print(".");
    delay(300);
  }
  Serial.println();
  Serial.print("Connected with IP: ");
  Serial.println(WiFi.localIP());
  Serial.println();
  config.api_key = API_KEY;
  config.database_url = DATABASE_URL;
  if (Firebase.signUp(&config, &auth, "", "")){
    Serial.println("ok");
    signupOK = true;
  }
  else{
    Serial.printf("%s\n", config.signer.signupError.message.c_str());
  }
  config.token_status_callback = tokenStatusCallback; //see addons/TokenHelper.h
  Firebase.begin(&config, &auth);
  Firebase.reconnectWiFi(true);

}

void loop() {

  Serial.print("Humidity1 ="); 
  Serial.print( dht1.readHumidity());
  Serial.println(" %");
  
  Serial.print("Temperature1, "); 
  Serial.print(dht1.readTemperature());
  Serial.println(" c");
  if (dht1.readTemperature() > 35.5){
  digitalWrite(buzzer, HIGH);
  delay(200);
  digitalWrite(buzzer, LOW);
  delay(200);  
  }
 
   Serial.print("Temperature = ");
    Serial.print(bmp.readTemperature());
    Serial.println(" *C");
    
    Serial.print("Pressure = ");
    Serial.print(bmp.readPressure());
    Serial.println(" Pa");
 
    Serial.print("Altitude = ");
    Serial.print(bmp.readAltitude());
    Serial.println(" meters");

    Serial.print("Pressure at sealevel (calculated) = ");
    Serial.print(bmp.readSealevelPressure());
    Serial.println(" Pa");

    Serial.print("Real altitude = ");
    Serial.print(bmp.readAltitude(101500));
    Serial.println(" meters");
    Serial.println();

    Serial.print("Smoke = ");
    Serial.print(analogRead(A0));
    Serial.println("AQI");
    Serial.println();
   
    if (Firebase.ready() && signupOK && (millis() - sendDataPrevMillis > 1000 || sendDataPrevMillis == 0)){
    sendDataPrevMillis = millis();

   
    if (Firebase.RTDB.setInt(&fbdo, "main/Humidity1", dht1.readHumidity())){
      Serial.println("PATH: " + fbdo.dataPath());
      Serial.println("TYPE: " + fbdo.dataType());
    }
    else {
      Serial.println("Failed REASON: " + fbdo.errorReason());
    }
     if (Firebase.RTDB.setInt(&fbdo, "main/Temperature1",dht1.readTemperature())){
      Serial.println("PATH: " + fbdo.dataPath());
      Serial.println("TYPE: " + fbdo.dataType());
    }
    else {
      Serial.println("Failed REASON: " + fbdo.errorReason());
    }
     if (Firebase.RTDB.setInt(&fbdo, "main/bmp",bmp.readTemperature())){
      Serial.println("PATH: " + fbdo.dataPath());
      Serial.println("TYPE: " + fbdo.dataType());
    }
    else {
      Serial.println("Failed REASON: " + fbdo.errorReason());
    }
      if (Firebase.RTDB.setInt(&fbdo, "main/pressure",bmp.readPressure())){
      Serial.println("PATH: " + fbdo.dataPath());
      Serial.println("TYPE: " + fbdo.dataType());
    }
    else {
      Serial.println("Failed REASON: " + fbdo.errorReason());
    }
     if (Firebase.RTDB.setInt(&fbdo, "main/Altitude",bmp.readAltitude())){
      Serial.println("PATH: " + fbdo.dataPath());
      Serial.println("TYPE: " + fbdo.dataType());
    }
    else {
      Serial.println("Failed REASON: " + fbdo.errorReason());
    }
     if (Firebase.RTDB.setInt(&fbdo, "main/Sealevel",bmp.readSealevelPressure())){
      Serial.println("PATH: " + fbdo.dataPath());
      Serial.println("TYPE: " + fbdo.dataType());
    }
    else {
      Serial.println("Failed REASON: " + fbdo.errorReason());
    }
     if (Firebase.RTDB.setInt(&fbdo, "main/Realaltitude",bmp.readAltitude(101500))){
      Serial.println("PATH: " + fbdo.dataPath());
      Serial.println("TYPE: " + fbdo.dataType());
    }
    else {
      Serial.println("Failed REASON: " + fbdo.errorReason());
    }
    if (Firebase.RTDB.setInt(&fbdo, "main/Smoke",analogRead(A0))){
      Serial.println("PATH: " + fbdo.dataPath());
      Serial.println("TYPE: " + fbdo.dataType());
    }
    else {
      Serial.println("Failed REASON: " + fbdo.errorReason());
    }
   }
    delay(5000);
 }

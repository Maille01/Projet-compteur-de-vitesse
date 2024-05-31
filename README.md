# Projet-compteur-de-vitesse
Les composants nécessaires au projet sont:
un capteur magnétique
,un afficheur oled
,une carte arduino

Le fonctionnement du circuit se déroule de la manière suivante:
Un aimant fixé à l'extrémité d'un rayon d'une roue de vélo (avec un diamètre définit) passe à chaque tour de roue devant le capteur magnétique accroché à la fourche.
Celui ci va donc pouvoir compter la fréquence de passage de l'aimant, ce qui se traduit en nombre de tour de par seconde. Et grâce à cette valeur on peut facilement calucler la vitesse: v=nb.diamètre.π .
La vitesse s'affiche par la suite sur l'afficheur oled en km/h.

Le code est le suivant: 

#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels
#define OLED_RESET    -1 // Reset pin # (or -1 if sharing Arduino reset pin)
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

#define WHEEL_CIRCUMFERENCE 2.1 // in meters, adjust this value based on your wheel size
#define SENSOR_PIN 2 // Digital pin connected to the sensor

volatile unsigned long lastTime = 0;
volatile unsigned long interval = 0;
volatile bool newData = false;

void setup() {
  pinMode(SENSOR_PIN, INPUT_PULLUP); // Setup the sensor pin with internal pullup
  attachInterrupt(digitalPinToInterrupt(SENSOR_PIN), sensorISR, FALLING); // Attach interrupt to sensor pin

  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) { // Initialize with the I2C addr 0x3C
    Serial.println(F("SSD1306 allocation failed"));
    for(;;);
  }
  display.display();
  delay(2000); // Pause for 2 seconds

  display.clearDisplay();
  display.setTextSize(2);      
  display.setTextColor(SSD1306_WHITE);  
}

void loop() {
  if (newData) {
    newData = false;
    float speed = calculateSpeed(interval);
    displaySpeed(speed);
  }
}

void sensorISR() {
  unsigned long currentTime = millis();
  interval = currentTime - lastTime;
  lastTime = currentTime;
  newData = true;
}

float calculateSpeed(unsigned long interval) {
  if (interval == 0) return 0; // Prevent division by zero
  float speed = (WHEEL_CIRCUMFERENCE / (interval / 1000.0)) * 3.6; // m/s to km/h
  return speed;
}

void displaySpeed(float speed) {
  display.clearDisplay();
  display.setCursor(0,0);
  display.print("Speed: ");
  display.print(speed);
  display.print(" km/h");
  display.display();
}

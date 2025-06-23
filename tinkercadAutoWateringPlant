#include <Adafruit_LiquidCrystal.h> 

int soil = 0;  
int temp = 0;  
int humidity = 0;  
int rain = 0;  

Adafruit_LiquidCrystal lcd(0);  

const int soilIN = A0;
const int tempIN = A1;
const int humidityIN = A2;
const int rainIN = 2;
const int pumpOUT = 3;
const int buzzerOUT = 8;
const int greenLED = 6;
const int redLED = 7;

float tempInCelsius = 0;
float humidityInPercentage = 0;

const int soilUpperBound = 800;  
const int soilLowerBound = 200;  
const int tempUpperBound = 30;   
const int humidityUpperBound = 70;  
const int humidityLowerBound = 20;  

#define NOTE_C4  262
#define NOTE_D4  294
#define NOTE_E4  330
#define NOTE_F4  349
#define NOTE_G4  392
#define NOTE_A4  440
#define NOTE_B4  494
#define NOTE_C5  523

int melody[] = {
  NOTE_C4, NOTE_C4, NOTE_G4, NOTE_G4, NOTE_A4, NOTE_A4, NOTE_G4,
  NOTE_F4, NOTE_F4, NOTE_E4, NOTE_E4, NOTE_D4, NOTE_D4, NOTE_C4
};

int noteDurations[] = {
  4, 4, 4, 4, 4, 4, 2,
  4, 4, 4, 4, 4, 4, 2
};

void setup() {
    lcd.begin(16, 2);
    lcd.print("Auto Watering");
    lcd.setCursor(0, 1);
    lcd.print("Plant System");
    delay(2000);
    lcd.clear();
  
    pinMode(soilIN, INPUT);
    pinMode(tempIN, INPUT);
    pinMode(humidityIN, INPUT);
    pinMode(rainIN, INPUT);
    pinMode(pumpOUT, OUTPUT);
    pinMode(buzzerOUT, OUTPUT);
    pinMode(greenLED, OUTPUT);
    pinMode(redLED, OUTPUT);
  
    Serial.begin(9600);
    delay(1000);  
}

void loop() {   
    
    soil = analogRead(soilIN);  
    humidity = analogRead(humidityIN);  
    temp = analogRead(tempIN);  
  

    float voltage = temp * (5.0 / 1023.0);  
    tempInCelsius = (voltage - 0.5) * 100;  


    humidityInPercentage = map(humidity, 0, 1023, 0, 100);  // Changing potentiometer scale from 0-1023 to 0-100
  

    if (humidityInPercentage > 50) {
        rain = 1;
    } else {
        rain = 0;
    }


    Serial.print("Temperature: ");
    Serial.print(tempInCelsius);
    Serial.println(" C");
    Serial.print("Moisture: ");
    Serial.println(soil);
  

  	lcd.begin(16, 2);
    lcd.setCursor(0, 0);
    lcd.print("Temp: ");
    lcd.print(tempInCelsius);
    lcd.print(" C");
    lcd.setCursor(0, 1);
    lcd.print("Moisture: ");
    lcd.print(soil);
    delay(2000);
    lcd.clear();
  

  

    if (rain == 1) {
        digitalWrite(greenLED, HIGH);
        digitalWrite(redLED, LOW);
        digitalWrite(buzzerOUT, LOW);
        digitalWrite(pumpOUT, LOW);
      	lcd.begin(16, 2);
        lcd.print("Water Pump OFF");
        delay(1000);
        lcd.clear();
        lcd.print("It's Raining");
    } else if (soil < soilLowerBound) {  
        digitalWrite(greenLED, LOW);
        digitalWrite(redLED, HIGH);
        playMelody();  // Play melody on buzzer
        digitalWrite(pumpOUT, HIGH);
      	lcd.begin(16, 2);
        lcd.print("Water Pump ON");
    } else if (soil < soilUpperBound && tempInCelsius > tempUpperBound) {  
        digitalWrite(greenLED, LOW);
        digitalWrite(redLED, HIGH);
        playMelody();  // Play melody on buzzer
        digitalWrite(pumpOUT, HIGH);
      	lcd.begin(16, 2);
        lcd.print("Water Pump ON");
    } else if (soil < soilUpperBound && humidityInPercentage < humidityLowerBound) {   
        digitalWrite(greenLED, LOW);
        digitalWrite(redLED, HIGH);
        playMelody();  // Play melody on buzzer
        digitalWrite(pumpOUT, HIGH);
      	lcd.begin(16, 2);
        lcd.print("Water Pump ON");
    } else {
        digitalWrite(greenLED, HIGH);
        digitalWrite(redLED, LOW);
        digitalWrite(buzzerOUT, LOW);
        digitalWrite(pumpOUT, LOW);
      	lcd.begin(16, 2);
        lcd.print("Water Pump OFF");
    }
  
    delay(1000);
    lcd.clear();
}

void playMelody() {
  for (int thisNote = 0; thisNote < sizeof(melody) / sizeof(int); thisNote++) {

    int noteDuration = 1000 / noteDurations[thisNote];
    tone(buzzerOUT, melody[thisNote], noteDuration);


    int pauseBetweenNotes = noteDuration * 1.30;
    delay(pauseBetweenNotes);


    noTone(buzzerOUT);
  }
}




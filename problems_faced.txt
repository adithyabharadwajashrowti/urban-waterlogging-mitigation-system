// Smart Water Purification System
// Arduino + TDS Sensor + LCD + Water Pump

#include <LiquidCrystal.h>

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

const int tdsPin = A0;
const int pumpPin = 7;

float voltage;
float tdsValue;

void setup() {
Serial.begin(9600);

lcd.begin(16, 2);

pinMode(pumpPin, OUTPUT);

lcd.print("Water System");
delay(2000);
lcd.clear();
}

void loop() {

// Read TDS sensor
int sensorValue = analogRead(tdsPin);

voltage = sensorValue * (5.0 / 1024.0);

// Approximate TDS calculation
tdsValue = (133.42 * voltage * voltage * voltage
- 255.86 * voltage * voltage
+ 857.39 * voltage) * 0.5;

// Display on LCD
lcd.setCursor(0, 0);
lcd.print("TDS:");
lcd.print((int)tdsValue);
lcd.print(" ppm");

// Water quality check
lcd.setCursor(0, 1);

if (tdsValue > 500) {

```
digitalWrite(pumpPin, HIGH);

lcd.print("Filtering... ");

Serial.println("Poor Water - Pump ON");
```

} else {

```
digitalWrite(pumpPin, LOW);

lcd.print("Water Safe ");

Serial.println("Water Safe - Pump OFF");
```

}

delay(1000);
}

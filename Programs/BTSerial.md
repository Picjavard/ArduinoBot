# Bluetooth-туннель телефона и ПК через Arduino

```
#include <SoftwareSerial.h>;

SoftwareSerial btSerial(2, 3);

void setup() {
  Serial.begin(9600);
  btSerial.begin(9600);

  delay(1000);
  
  Serial.println("Connected Arduino to PC");
  btSerial.println("Connected Arduino to Bluetooth");
}

void loop() {
  if (btSerial.available()) {
    char buff = btSerial.read();
    Serial.println(buff);
  }
  if (Serial.available()) {
    char buff = Serial.read();
    btSerial.println(buff);
  }

}
```

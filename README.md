# ArduinoBot

```C++
#include <SoftwareSerial.h> //подключение библиотеки для Bluetooth-модуля
SoftwareSerial BT(2, 3);    //BT на 2 (TX) и 3 (RX) пины

#include <ServoSmooth.h>  //подключение библиотеки для серво
ServoSmooth S1;           // Серво 1
ServoSmooth S2;           // Серво 2
ServoSmooth S3;           // Серво 3
ServoSmooth S4;           // Серво 4
ServoSmooth S5;           // Серво 5

int posS5 = 0;  // переменная для указания позиции серво доп.

// Пины драйвера моторов
int IN1 = 4;
int IN2 = 7;
int IN3 = 8;
int IN4 = 12;

void setup() {
  BT.begin(9600);  //Соединение с BT-модулем

  SetupServos();  // Настройка серво
  SetupMotors();  // Настройка моторов

  delay(1000);
}

void loop() {
  if (BT.available() > 0) {
    char buff = BT.read();  // Считываем символ пришедший от BT

    switch (buff) {
      //Моторы
      case 'Q':
        MotorL_Forward();
        break;
      case 'q':
        MotorL_Stop();
        break;

      case 'A':
        MotorL_Backward();
        break;
      case 'a':
        MotorL_Stop();
        break;

      case 'E':
        MotorR_Forward();
        break;
      case 'e':
        MotorR_Stop();
        break;

      case 'D':
        MotorR_Backward();
        break;
      case 'd':
        MotorR_Stop();
        break;


      //Серво 1 плечо
      case 'U':
        S1.setTarget(170);
        break;
      case 'J':
        S1.setTarget(10);
        break;

      //Серво 2 плечо
      case 'I':
        S2.setTarget(170);
        break;
      case 'K':
        S2.setTarget(10);
        break;

      //Серво кисть
      case 'R':
        S3.setTarget(Serial.parseInt());
        break;

      //Серво клешня
      case 'F':
        S4.setTarget(170);
        break;
      case 'f':
        S4.setTarget(10);
        break;

      //Серво доп.
      case 'T':
        if ((posS5 >= 10) && (posS5 <= 170)) posS5 += 5;
        S5.setTarget(posS5);
        break;
      case 'G':
        if ((posS5 >= 10) && (posS5 <= 170)) posS5 -= 5;
        S5.setTarget(posS5);
        break;


      default:
        break;
    }
  }
}

// Настроить Моторы
void SetupMotors() {
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  //Выключить все
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

// Настроить серво
void SetupServos() {
  //Прикрепление всех серв к пинам:
  S1.attach(5, 90);
  S1.smoothStart();
  S2.attach(6, 90);
  S2.smoothStart();
  S3.attach(9, 90);
  S3.smoothStart();
  S4.attach(10, 90);
  S4.smoothStart();
  S5.attach(11, 90);
  S5.smoothStart();
}

void MotorL_Forward() {
  //лев. мотор
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
}

void MotorL_Backward() {
  //лев. мотор
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
}

void MotorL_Stop() {
  //лев. мотор
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
}

void MotorR_Forward() {
  //лев. мотор
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void MotorR_Backward() {
  //лев. мотор
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void MotorR_Stop() {
  //лев. мотор
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}


```

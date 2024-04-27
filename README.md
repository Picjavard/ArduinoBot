# ArduinoBot

```C++

#include <SoftwareSerial.h>  //подключение библиотеки для Bluetooth-модуля
SoftwareSerial BT(2, 3);     //BT на 2 (TX) и 3 (RX) пины

#include <ServoSmooth.h>  //подключение библиотеки для серво
ServoSmooth S1;           // Серво 1
ServoSmooth S2;           // Серво 2
//ServoSmooth S3;           // Серво 3
ServoSmooth S4;           // Серво 4
ServoSmooth S5;           // Серво 5


int posS1 = 90;  // переменная для указания позиции серво
int posS2 = 90;  // переменная для указания позиции серво
//int posS3 = 90;  // переменная для указания позиции серво
int posS4 = 90;  // переменная для указания позиции серво
int posS5 = 90;  // переменная для указания позиции серво доп.


// Пины драйвера моторов
int IN1 = 4;
int IN2 = 7;
int IN3 = 8;
int IN4 = 12;

#define TIMEOUT 100  // таймаут в миллисекундах на отработку неправильно посланных данных
int intValue;
char header;
boolean recievedFlag, startParse;
unsigned long parseTime;

void setup() {
  BT.begin(9600);  //Соединение с BT-модулем
  Serial.begin(9600);
  SetupServos();  // Настройка серво
  SetupMotors();  // Настройка моторов

  delay(1000);
}

void parsing() {
  if (BT.available() > 0) {      // если есть что то на вход
    char thisChar = BT.read();   // принимаем байт
    if (startParse) {            // если парсим
      if (!isDigit(thisChar)) {  // если приходит НЕ ЦИФРА
        switch (header) {        // раскидываем по переменным
          case 'U':              // Серво плечо 1
            posS1 = intValue;
            break;

          case 'I':  // Серво плечо 2
            posS2 = intValue;
            break;

          case 'F':  // Серво клешня
            posS4 = intValue;
            break;

          case 'L':  // Серво плечо 3
            posS5 = intValue;
            break;

            // case 'P':
            //   if ((posS5 >= 10) && (posS5 < 170)) posS5 += 10;

            //   break;

            // case 'p':
            //   if ((posS5 > 10) && (posS5 <= 170)) posS5 -= 10;

            //   break;
        }
        recievedFlag = true;                          // данные приняты
        startParse = false;                           // парсинг завершён
      } else {                                        // если принятый байт всё таки цифра
        intValue = intValue * 10 + (thisChar - '0');  // с каждым принятым число растёт слева направо
      }
    }
    if (isAlpha(thisChar) && !startParse) {  // если префикс БУКВА и парсинг не идёт
      header = thisChar;                     // запоминаем префикс
      intValue = 0;                          // обнуляем
      startParse = true;                     // флаг на парсинг
      parseTime = millis();                  // запоминаем таймер
      switch (header) {                      // раскидываем по переменным
        case 'Q':                            // если заголовок а
          MotorL_Forward();
          break;
        case 'q':  // если заголовок а
          MotorL_Stop();
          break;
        case 'A':  // если заголовок а
          MotorL_Backward();
          break;
        case 'a':  // если заголовок а
          MotorL_Stop();
          break;
        case 'E':  // если заголовок а
          MotorR_Forward();
          break;
        case 'e':  // если заголовок а
          MotorR_Stop();
          break;
        case 'D':  // если заголовок а
          MotorR_Backward();
          break;
        case 'd':  // если заголовок а
          MotorR_Stop();
          break;

        case 'V':  // если заголовок а
          posS1 = 30;
          posS2 = 90;
          posS5 = 30;
          posS4= 90; //клешня
          break;
        case 'B':  // если заголовок а
          posS1 = 90;
          posS2 = 90;
          posS5 = 120;
          break;
        case 'N':  // если заголовок а
          posS1 = 45;
          posS2 = 90;
          posS5 = 90;
          break;
      }
    }
  }
  if (startParse && (millis() - parseTime > TIMEOUT)) {
    startParse = false;  // парсинг завершён по причине таймаута
  }
}

void loop() {
  S1.tick();
  S2.tick();
 // S3.tick();
  S4.tick();
  S5.tick();

  parsing();

  if (recievedFlag) {
    // выводим в порт для отладки
    S1.setTargetDeg(posS1);
    S2.setTargetDeg(posS2);
   // S3.setTargetDeg(posS3);
    S4.setTargetDeg(posS4);
    S5.setTargetDeg(posS5);
    recievedFlag = false;
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
  S1.setAutoDetach(false);
  S2.attach(6, 90);
  S2.setAutoDetach(false);
  // S3.attach(9, 90);
  // S3.setAutoDetach(false);
  S4.attach(10, 90);
  S4.setAutoDetach(false);
  S5.attach(11, 120);
  S5.setAutoDetach(false);

  S1.smoothStart();
  S2.smoothStart();
  //S3.smoothStart();
  S4.smoothStart();
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

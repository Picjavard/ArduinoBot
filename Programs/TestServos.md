```C
#include <Servo.h> //подключение библиотеки

Servo S1;   // Серво 1
Servo S2;   // Серво 2
Servo S3;  // Серво 3
Servo S4;  // Серво 4

int pos = 0;    

// Мин и Макс углы поворота серво 1
int S1_Min = 10;
int S1_Max = 170;
int S1_Start = 80;

// Мин и Макс углы поворота серво 2
int S2_Min = 10;
int S2_Max = 170;
int S2_Start = 80;

// Мин и Макс углы поворота серво 3
int S3_Min = 10;
int S3_Max = 170;
int S3_Start = 80;

// Мин и Макс углы поворота серво 4
int S4_Min = 10;
int S4_Max = 170;
int S4_Start = 80;


void setup() {
  //Прикрепление всех серв к пинам:
  S1.attach(11);
  S2.attach(10);
  S3.attach(9);  
  S4.attach(6);  

  //Проверка всех серв
  servosTest();
}

void loop() {
  
}

//Функция для тестирования серводвигателя
//серво проходит нач.угол -> мин.угол -> макс.угол -> нач.угол
void servoTest(Servo s, int s_min, int s_max, int s_start){
  for (pos = s_start; pos >= s_min; pos -= 1) { 
    s.write(pos);              
    delay(15);                       
  }
  for (pos = s_min; pos <= s_max; pos += 1) { 
    s.write(pos);              
    delay(15);                       
  }
  for (pos = s_max; pos >= s_start; pos -= 1) { 
    s.write(pos);              
    delay(15);                       
  }
  delay(200);
}

void servosTest(){
  // Параметры <Servo>, <Минимальный угол> <Максимальный угол> <Угол по умолчанию>
  servoTest(S1, S1_Min, S1_Max, S1_Start);
  servoTest(S2, S2_Min, S2_Max, S2_Start);
  servoTest(S3, S3_Min, S3_Max, S3_Start);
  servoTest(S4, S4_Min, S4_Max, S4_Start);
}
```

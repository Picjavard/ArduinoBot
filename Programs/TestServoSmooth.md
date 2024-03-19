# Программа для одновременного теста 4х серво с использованием библиотеки ServoSmooth (без delay(), с таймерами)

```C++
#include <ServoSmooth.h>//подключение библиотеки для серво

ServoSmooth S1;   // Серво 1
ServoSmooth S2;   // Серво 2
ServoSmooth S3;  // Серво 3
ServoSmooth S4;  // Серво 4

#include <TimerMs.h> // библиотека таймера

// (период, мс), (0 не запущен / 1 запущен), (режим: 0 период / 1 таймер)
TimerMs tmr1(12000, 1, 0);  //основной таймер для тестового режима
TimerMs tmrDir(3000, 1, 1); //таймер на смену угла поворота

bool isTest = true; //флаг тестового режима

int pos = 0;  // переменная для указания позиции 

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

  //Запуск таймера для смены позиции
  tmrDir.setPeriodMode(); 
}

void loop() {
  S1.tick();
  S2.tick();
  S3.tick();
  S4.tick();

  // Тестовый режим на 12 сек:
  if(isTest) servosTest();
}

void servosTest(){
  //Проверка таймера на окончание тестового режима:
  if(tmr1.tick()){
    isTest = !isTest; //Выключить тестовый режим
  }

  //Провекрка таймера на смену позиции:
  if(tmrDir.tick()){
    pos++;  //Сменить позицию на следующую

    if (pos == 4) pos = 1;  // Если позиция 4 - вернуть на 1

    if(pos == 1)  //1 позиция
    {
      S1.setTargetDeg(S1_Start);
      S2.setTargetDeg(S2_Start);
      S3.setTargetDeg(S3_Start);
      S4.setTargetDeg(S4_Start);
    }
    else if(pos == 2) //2 позиция
    {
      S1.setTargetDeg(S1_Min);
      S2.setTargetDeg(S2_Min);
      S3.setTargetDeg(S3_Min);
      S4.setTargetDeg(S4_Min);
    }
    else if(pos == 3) //3 позиция
    {
      S1.setTargetDeg(S1_Max);
      S2.setTargetDeg(S2_Max);
      S3.setTargetDeg(S3_Max);
      S4.setTargetDeg(S4_Max);
    }
  }
}
```

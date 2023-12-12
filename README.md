# ArduinoBot

```

int IN1 = 2;
int IN2 = 3;
int IN3 = 4;
int IN4 = 5;

void setup() {
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);

  MotorTest();
}

void loop() {
  
}

void MotorTest() {
  // Проверка 1 мотора направление вперед
  digitalWrite(IN1, HIGH);
  delay(500);
  digitalWrite(IN1, LOW);
  delay(500);

  // Проверка 1 мотора направление обратное
  digitalWrite(IN2, HIGH);
  delay(500);
  digitalWrite(IN2, LOW);
  delay(500);

  // Проверка 2 мотора направление вперед
  digitalWrite(IN3, HIGH);
  delay(500);
  digitalWrite(IN3, LOW);
  delay(500);

  // Проверка 2 мотора направление обратное
  digitalWrite(IN4, HIGH);
  delay(500);
  digitalWrite(IN4, LOW);
  delay(500);
}

```

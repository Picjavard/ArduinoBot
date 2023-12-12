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
  // Проверка моторов
  MoveForward();
  delay(500);

  MoveBackward();
  delay(500);

  MoveRight();
  delay(500);

  MoveLeft();
  delay(500);

  Stop();
  delay(500);
}

// 1
void MoveForward() {
  //лев. мотор
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  //прав. мотор
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

// 2
void MoveBackward() {
  //лев. мотор
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  //прав. мотор
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

// 3
void MoveRight() {
  //лев. мотор ^^^
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  //прав. мотор VVV
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

// 4
void MoveLeft() {
  //лев. мотор VVV
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  //прав. мотор ^^^
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

// 5
void Stop() {
  //лев. мотор x
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  //прав. мотор x
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
```

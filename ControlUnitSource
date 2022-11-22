#include <SoftwareSerial.h>
#include <Servo.h>

// DRON
// ① ②
// ③ ④
//
// Wing
// L | R
//
// Wheel
// ① ②
// ③ ④

/* 드론 모터 ESC 정의 */
Servo DronMoter1;
Servo DronMoter2;
Servo DronMoter3;
Servo DronMoter4;

/* 날개 서보 정의 */
Servo WingL;
Servo WingR;

/* 속도 변수 */
int SpeedVal = 35;
int SetVal = 35;

/* 날개 모터 값 변수 */
int WingLVal = 0;
int WingRVal = 0;
bool WingState = false;

/* 전후 밸런스 조정 변수 */
const int FrontCorrectionFactor = 0;
const int BackCorrectionFactor = 0; //<~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~수정필요

/* 좌우 균형 조절 */
const int BalanceVal = 5;

/* 날개 서보 세팅 변수 */
const int FoldingRadL = 0; // 기본값
const int FoldingRadR = 0; // 기본값
const int FoldMovRadL = 0; // 각도변위
const int FoldMovRadR = 0; // 각도변위 //<~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~수정필요

/* 드론 모터 셋업 스피드 */
const int MinSpeed = 35; // 최소 동작 값
const int MaxSpeed = 165; // 최대 동작 값

void setup() {
  Serial.begin(9600);
  Serial1.begin(9600);

  // 8~11 드론 모터 배정
  DronMoter1.attach(8);
  DronMoter1.write(MinSpeed);
  DronMoter2.attach(9);
  DronMoter2.write(MinSpeed);
  DronMoter3.attach(10);
  DronMoter3.write(MinSpeed);
  DronMoter4.attach(11);
  DronMoter4.write(MinSpeed);

  // 12~13 날개 L R 배정
  WingL.attach(12);
  WingL.write(FoldingRadL);
  WingR.attach(13);
  WingR.write(FoldingRadR);

  // RC 차량 바퀴 출력 설정
  pinMode(7, OUTPUT);
  pinMode(6, OUTPUT);
  pinMode(5, OUTPUT);
  pinMode(4, OUTPUT);

  
  delay(3000);
}

void loop() {
  /* 시리얼 제어 코드~~~~~~~~~~~~~~~~~~~~~~~~~~~ */
  if (Serial.available()) {
    char temp = Serial.read();

    /* 드론 모터 속력 제어 */
    if (temp == 'u') {
      SetVal += 10;
    }
    else if (temp == 'd') {
      SetVal -= 10;
    }
    
    if (temp == 'x') {
      SetVal = 35;
    }

    if (temp == 'q') {
      SetVal += 1;
    }
    else if (temp == 'e') {
      SetVal -= 1;
    }

    /* 날개 제어 */
    if (temp == 'o') { // 날개 폄
      WingLVal = FoldMovRadL;
      WingRVal = FoldMovRadR;
      WingState = true;
    }
    else if (temp == 'c') { // 날개 접음
      WingLVal = FoldingRadL;
      WingRVal = FoldingRadR;
      WingState = false;
    }

    if (temp == 'x') { // 비상 정지
      SetVal = MinSpeed;
    }

    /* 차량 동력 제어 */
    if (temp == 'w') { // 전진
      analogWrite(7, 0);
      analogWrite(6, 250);
      analogWrite(5, 0);
      analogWrite(4, 250);
    }
    else if (temp == 's') { // 후진
      analogWrite(7, 0);
      analogWrite(6, 0);
      analogWrite(5, 0);
      analogWrite(4, 0);
    }

    /* 드론 모터 최대 최소 속력 세팅 */
    if ((SetVal + FrontCorrectionFactor) <= MaxSpeed &&
    (SetVal + FrontCorrectionFactor) >= MinSpeed &&
    (SetVal + BackCorrectionFactor) <= MaxSpeed &&
    (SetVal + BackCorrectionFactor) >= MinSpeed) {
      SpeedVal = SetVal;
    }
    else {
      SetVal = SpeedVal;
    }

    /* 모터값 입력 */
    DronMoter1.write(SpeedVal + FrontCorrectionFactor + BalanceVal);
    DronMoter2.write(SpeedVal + FrontCorrectionFactor);
    DronMoter3.write(SpeedVal + BackCorrectionFactor + BalanceVal);
    DronMoter4.write(SpeedVal + BackCorrectionFactor);

    /* 날개 서보 입력 */
    WingL.write(WingLVal);
    WingR.write(WingRVal);

    Serial.print("Now Speed : ");
    Serial.println(SpeedVal);
  }
  /* 여기까지~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ */

  
  /* 블루투스 제어 코드~~~~~~~~~~~~~~~~~~~~~~~~~~~ */
  if (Serial1.available()) {
    char temp = Serial1.read();

    /* 드론 모터 속력 제어 */
    if (temp == 'u') {
      SetVal += 10;
    }
    else if (temp == 'd') {
      SetVal -= 10;
    }

    if (temp == 'x') {
      SetVal = 35;
    }

    /* 날개 제어 */
    if (temp == 'o') { // 날개 폄
      WingLVal = FoldMovRadL;
      WingRVal = FoldMovRadR;
      WingState = true;
    }
    else if (temp == 'c') { // 날개 접음
      WingLVal = FoldingRadL;
      WingRVal = FoldingRadR;
      WingState = false;
    }

    /* 차량 동력 제어 */
    if (temp == 'w') { // 전진
      analogWrite(7, 0);
      analogWrite(6, 250);
      analogWrite(5, 0);
      analogWrite(4, 250);
    }
    else if (temp == 's') { // 후진
      analogWrite(7, 0);
      analogWrite(6, 0);
      analogWrite(5, 0);
      analogWrite(4, 0);
    }

    /* 드론 모터 최대 최소 속력 세팅 */
    if ((SetVal + FrontCorrectionFactor) <= MaxSpeed &&
    (SetVal + FrontCorrectionFactor) >= MinSpeed &&
    (SetVal + BackCorrectionFactor) <= MaxSpeed &&
    (SetVal + BackCorrectionFactor) >= MinSpeed) {
      SpeedVal = SetVal;
    }
    else {
      SetVal = SpeedVal;
    }

    /* 모터값 입력 */
    DronMoter1.write(SpeedVal + FrontCorrectionFactor);
    DronMoter2.write(SpeedVal + FrontCorrectionFactor);
    DronMoter3.write(SpeedVal + BackCorrectionFactor);
    DronMoter4.write(SpeedVal + BackCorrectionFactor);

    /* 날개 서보 입력 */
    WingL.write(WingLVal);
    WingR.write(WingRVal);

    Serial1.print("Now Speed : ");
    Serial1.println(SpeedVal);
  }
  
  delay(100);
}

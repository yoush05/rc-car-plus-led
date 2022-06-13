#include <SoftwareSerial.h> 

SoftwareSerial BTSerial(12, 13); 

int motor1PinA = 2 ; int motor1PinB = 3 ; int enablelPin =11 ;
int motor2PinA = 4 ; int motor2PinB = 5 ; int enableRpin = 10 ;
int led1 = 7 ; int led2 = 8;

char in; 
void setup() 
{ 
  BTSerial.begin(9600);
  Serial.begin(9600);
    pinMode(led1, OUTPUT);
    pinMode(led2, OUTPUT);


   pinMode(motor1PinA, OUTPUT);
   pinMode(motor1PinB, OUTPUT);
   pinMode(motor2PinA, OUTPUT);
   pinMode(motor2PinB, OUTPUT);
   pinMode(enableRpin, OUTPUT);
   pinMode(enablelPin, OUTPUT);
   analogWrite(enableRpin, 250);
   analogWrite(enablelPin, 250);
}

void loop() 
{
if (BTSerial.available())
{ 
in =BTSerial.read();
Serial.write(in);
}
if (Serial.available()) 
{  
BTSerial.write(Serial.read());
Serial.print("data =");
Serial.println(in);
}    
switch(in){
case 'F':Forward(); break;
case 'R': Right(); break; 
case 'S': Stop(); break;
case 'L': Left(); break;
case 'B': Back(); break;
case 'G': FowardLeft(); break;
case 'I' : FowardRight(); break;
         } 
  }  

void Back(){
  digitalWrite(led1, LOW); 
digitalWrite(motor1PinA, HIGH); 
digitalWrite(motor1PinB, LOW); 
digitalWrite(motor2PinA, HIGH); 
digitalWrite(motor2PinB, LOW); 
digitalWrite(led2, HIGH); 
} 

void Right(){
digitalWrite(motor1PinA, HIGH); 
digitalWrite(motor1PinB, HIGH); 
digitalWrite(motor2PinA, LOW); 
digitalWrite(motor2PinB, HIGH); 

}

void Stop(){
  digitalWrite(led1, LOW); 
  digitalWrite(led2, LOW); 
digitalWrite(motor1PinA, HIGH); 
digitalWrite(motor1PinB,HIGH); 
digitalWrite(motor2PinA,HIGH); 
digitalWrite(motor2PinB,HIGH); 
} 

void Left(){
digitalWrite(motor1PinA, LOW); 
digitalWrite(motor1PinB, HIGH); 
digitalWrite(motor2PinA, LOW); 
digitalWrite(motor2PinB, LOW); 
} 

void Forward(){
  digitalWrite(led2, LOW); 
digitalWrite(motor1PinA, LOW); 
digitalWrite(motor1PinB, HIGH); 
digitalWrite(motor2PinA, LOW); 
digitalWrite(motor2PinB, HIGH); 
digitalWrite(led1, HIGH); 

}

void FowardLeft(){
analogWrite(enablelPin,100); 
analogWrite(enableRpin, 200); 
digitalWrite(motor1PinA, LOW); 
digitalWrite(motor1PinB, HIGH); 
digitalWrite(motor2PinA, LOW); 
digitalWrite(motor2PinB, HIGH); 
}

void FowardRight(){
analogWrite(enablelPin,200); 
analogWrite(enableRpin, 100); 
digitalWrite(motor1PinB, LOW); 
digitalWrite(motor1PinA, HIGH); 
digitalWrite(motor2PinB, LOW); 
digitalWrite(motor2PinA, HIGH); 
}


이 프로그램의 지난 rc카 코딩과 다른점이 있다면 Forward 와 Back와 Stop 부분에 있다. 
이 코딩은 rc카의 조종 방향에 따라 서로 다른 led가 켜지게 만드는 프로그램이다. 
앞으로 갈때는 led1을 키고 뒤로 갈때는 led2를 킨다. 

void Forward 에서 맨 첫줄에 led2를 끈것은 후진에서 전진으로 바뀌었을때 기존의 led2를 꺼주기 위한 것이다. 
void Back 에서도 같은 이유로 첫줄에 led1을 꺼줬다.

다른 이동에서는 왜 이 코드를 적지 않은 이유는 모든 이동이 직진 또는 후진 Base 로 가기 때문에 같은 계열로 보고 하지 않았다. 

회로는 기존의 rc카 회로에서 led 회로를 추가했다. (led회로만 추가해서 첨부함)
각각의 파란선과 노란선은 코드에서 볼수있듯이 아두이노에서 7,8번에 연결했다. 
그리고 gnd 로 가는 전류를 공유해서 사용했다. 

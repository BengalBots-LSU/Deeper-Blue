#include <PS4Controller.h>

unsigned long lastTimeStamp = 0;
int RightForward = 27;
int RightBack = 26;

int LeftForward = 12;
int LeftBack = 14;

int RightSpeedPin = 33;
int LeftSpeedPin = 25;
int RightSpeedChan = 0;
int LeftSpeedChan = 1;

int Rtrigger = 0;
int Ltrigger = 0;
int steer = 0;

int stickDrift = 15;

void notify() {
  char messageString[200];
  sprintf(messageString, "%4d,%4d,%4d,%4d,%3d,%3d,%3d,%3d,%3d,%3d,%3d,%3d,%3d,%3d,%4d,%4d,%3d,%3d,%3d,%3d,%3d,%3d,%3d,%3d",
          PS4.LStickX(),
          PS4.LStickY(),
          PS4.RStickX(),
          PS4.RStickY(),
          PS4.Left(),
          PS4.Down(),
          PS4.Right(),
          PS4.Up(),
          PS4.Square(),
          PS4.Cross(),
          PS4.Circle(),
          PS4.Triangle(),
          PS4.L1(),
          PS4.R1(),
          PS4.L2(),
          PS4.R2(),
          PS4.Share(),
          PS4.Options(),
          PS4.PSButton(),
          PS4.Touchpad(),
          PS4.Charging(),
          PS4.Audio(),
          PS4.Mic(),
          PS4.Battery());

  //Only needed to print the message properly on serial monitor. Else we dont need it.
  //if (millis() - lastTimeStamp > 50) {
  //  Serial.println(messageString);
  //  lastTimeStamp = millis();
  //}
}

void onConnect() {
  Serial.println("Connected!.");
}

void onDisConnect() {
  Serial.println("Disconnected!.");
}

void setup() {
  pinMode(RightForward, OUTPUT);
  pinMode(RightBack, OUTPUT);
  pinMode(LeftForward, OUTPUT);
  pinMode(LeftBack, OUTPUT);

  ledcSetup(RightSpeedChan, 5000, 8); 
  ledcSetup(LeftSpeedChan, 5000, 8);
  ledcAttachPin(RightSpeedPin, RightSpeedChan);
  ledcAttachPin(LeftSpeedPin, LeftSpeedChan);

  Serial.begin(115200);
  PS4.attach(notify);
  PS4.attachOnConnect(onConnect);
  PS4.attachOnDisconnect(onDisConnect);
  PS4.begin();
  Serial.println("Ready.");
}

void loop() {

  steer = PS4.RStickX();
  Rtrigger = PS4.R2();
  Ltrigger = PS4.L2();
  Serial.print(steer);

  if(steer <= 127 && steer > stickDrift){//right Turn
    int val = map(steer, 0, 127, 255, 0);
    if(abs(Rtrigger) == 1){
      digitalWrite(RightForward, HIGH);
      digitalWrite(LeftForward, HIGH);
      digitalWrite(RightBack, LOW);
      digitalWrite(LeftBack, LOW);

      ledcWrite(RightSpeedChan, val);
      ledcWrite(LeftSpeedChan, 255);
      Serial.print("Right Forward: ");
      Serial.println(val);
    }
    else if(abs(Ltrigger) == 1){
      digitalWrite(RightForward, LOW);
      digitalWrite(LeftForward, LOW);
      digitalWrite(RightBack, HIGH);
      digitalWrite(LeftBack, HIGH);

      ledcWrite(RightSpeedChan, val);
      ledcWrite(LeftSpeedChan, 255);
      Serial.print("Right Backwards: ");
      Serial.println(val);
    }
    else{
      ledcWrite(RightSpeedChan, 0);
      ledcWrite(LeftSpeedChan, 0);
      Serial.println("Right Stopped");
    }
  }
  else if(steer >= -127 && steer < -1*stickDrift){//left Turn
    int val = map(steer, 0, -127, 255, 0);
    if(abs(Rtrigger) == 1){
      digitalWrite(RightForward, HIGH);
      digitalWrite(LeftForward, HIGH);
      digitalWrite(RightBack, LOW);
      digitalWrite(LeftBack, LOW);

      ledcWrite(RightSpeedChan, 255);
      ledcWrite(LeftSpeedChan, val);
      Serial.print("Left Forward: ");
      Serial.println(val);
    }
    else if(abs(Ltrigger) == 1){
      digitalWrite(RightForward, LOW);
      digitalWrite(LeftForward, LOW);
      digitalWrite(RightBack, HIGH);
      digitalWrite(LeftBack, HIGH);

      ledcWrite(RightSpeedChan, 255);
      ledcWrite(LeftSpeedChan, val);
      Serial.print("Left Backwards: ");
      Serial.println(val);
    }
    else{
      ledcWrite(RightSpeedChan, 0);
      ledcWrite(LeftSpeedChan, 0);
      Serial.println("Left Stopped");
    }
  }
  else{
    if (abs(Rtrigger) == 1){
      digitalWrite(RightForward, HIGH);
      digitalWrite(LeftForward, HIGH);
      digitalWrite(RightBack, LOW);
      digitalWrite(LeftBack, LOW);
      ledcWrite(RightSpeedChan, 255);
      ledcWrite(LeftSpeedChan, 255);
      Serial.println("Straight Forward");
    }
    else if (abs(Ltrigger) == 1){
      digitalWrite(RightForward, LOW);
      digitalWrite(LeftForward, LOW);
      digitalWrite(RightBack, HIGH);
      digitalWrite(LeftBack, HIGH);
      ledcWrite(RightSpeedChan, 255);
      ledcWrite(LeftSpeedChan, 255);
      Serial.println("Straight Backward");
    }
    else{
      ledcWrite(RightSpeedChan, 0);
      ledcWrite(LeftSpeedChan, 0);
      Serial.println("Stopped");
    }
  }
}

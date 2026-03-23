//requires Dabble app
#include <Dabble.h>
// Enable the Motor Control module
#define CUSTOM_SETTINGS
#define INCLUDE_GAMEPAD_MODULE   // or INCLUDE_MOTORCONTROL_MODULE depending on which you use
// Motor pins
int lBwd = 5;
int lFwd = 11;
int rBwd = 9;
int rFwd = 10;
// Sensor pins and thresholds
int rSans=A1; //left
int lSans=A0; //right
int rB=300; // left sensor
int lB=400; // right sensor
int speed=150; // speed variable
char* mode = "control"; // variable whose definition determines the mode




void setup() {
  pinMode(rFwd, OUTPUT);
  pinMode(rBwd, OUTPUT);
  pinMode(lFwd, OUTPUT);
  pinMode(lBwd, OUTPUT);
  Serial.begin(9600);
  Dabble.begin(9600);   // HC-05 default baud rate
}


// --- Motor functions ---
void left() { // go left
  analogWrite(rBwd, 0);
  analogWrite(lFwd, 0);
  analogWrite(lBwd, speed);
  analogWrite(rFwd, speed);
}


void right() { // go right
  analogWrite(lBwd, 0);
  analogWrite(rFwd, 0);
  analogWrite(rBwd, speed);
  analogWrite(lFwd, speed);
}


void forward() { // go forewards
  analogWrite(rBwd, 0);
  analogWrite(lBwd, 0);
  analogWrite(rFwd, speed);
  analogWrite(lFwd, speed);
}


void backward() { // go backwards
  analogWrite(rFwd, 0);
  analogWrite(lFwd, 0);
  analogWrite(rBwd, speed);
  analogWrite(lBwd, speed);
}


void stopMotors() { // halt
  analogWrite(rFwd, 0);
  analogWrite(rBwd, 0);
  analogWrite(lFwd, 0);
  analogWrite(lBwd, 0);
}


void loop() {
  Dabble.processInput();
  if (GamePad.isCrossPressed()) {mode="line";} // if X is pressed switch to line tracking
  if (GamePad.isTrianglePressed()) {mode="control";} // if Triangle is pressed switch to Bluetooth control
    // if statements checking the status of various buttons
  if (mode=="control") { // if in Bluetooth mode
    if (GamePad.isUpPressed()) forward(); // go forwards if up pressed
    if (GamePad.isDownPressed()) backward(); // go backwards if backwards pressed
    if (GamePad.isLeftPressed()) left(); // go left if left pressed
    if (GamePad.isRightPressed()) right(); // go right if right pressed
  }
 
  if (mode == "line"){
    // go forward
    analogWrite(rBwd, 0);
    analogWrite(lBwd, 0);
    analogWrite(rFwd, speed);
    analogWrite(lFwd, speed);
    while (true) {
      Dabble.processInput();
      if (analogRead(rSans)<rB && lB>analogRead(lSans)){
        forward(); // if no sensor reads black go forwards
      }


      if (analogRead(rSans)>rB && analogRead(rSans)>analogRead(lSans)){
        right(); // if right sensor reads black go right
      }


      if (analogRead(lSans)>lB && analogRead(rSans)<analogRead(lSans)){
        left(); // if left sensor reads black go left
      }


      if (GamePad.isTrianglePressed()) {
        mode="control";
        break; // if Triangle is pressed switch to Bluetooth control and exit the while loop
      }
    }
  }
}

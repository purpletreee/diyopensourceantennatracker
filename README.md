# diyopensourceantennatracker
diy opensource antenna tracker for RC airplanes / drones ...

Still in progress

code: 

#include <Servo.h>
//Coordinates of ground station
float latA = xx.xxxxx;
float longA = xx.xxxxx;

//Coordinates of plane / quad ...
//Live transmision of coordinates from UAV has to be integrated here to change the values
float latB = xx.xxxxx;
float longB = xx.xxxxx;

//vector variables
float vecAB1 = 0;
float vecAB2 = 0;
float vecAC1 = 0;
float vecAC2 = 0;

float skalAB = 0;

float betAB = 0;
float betAC = 0;
float betges = 0;

float cosa = 0;

float alpha = 0;
float alphad = 0;

float latC = 0;
float longC = 0;

Servo servoeins;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  //servo on pin 8
  servoeins.attach(8);
  latC = latA + 10;
  longC = longA;  
  //Vector AB
  //Serial.println("AB:");
  //Serial.print("AB1:(");
  vecAB1 = latB - latA;
  //Serial.print(vecAB1,10);
  //Serial.print("|");
  vecAB2 = longB - longA;
  //Serial.print(vecAB2,10);
  //Serial.print(")");
  
  //Vector AC
  //Serial.println("AC:");
  //Serial.print("AC1:(");
  vecAC1 = latC - latA;
  //Serial.print(vecAC1,10);
  //Serial.print("|");
  vecAC2 = longC - longA;
  //Serial.print(vecAC2,10);
  //Serial.print(")");

  //scalar (not sure if this is the corect expresion in english) AB AC
  //Serial.println("skalabac:");
  skalAB = vecAB1 * vecAC1 + vecAB2 * vecAC2;
  //Serial.print(skalAB, 10);

  //amount/sum of AB
  //Serial.println("Betrag AB:");
  betAB = sqrt(pow(vecAB1, 2) + pow(vecAB2,2));
    
  //Serial.print(betAB, 10);

  //Betrag AC
 // Serial.println("Betrag AC:");
  betAC = sqrt(pow(vecAC1, 2) + pow(vecAC2,2));
  //Serial.print(betAC, 10);

  //cosalpha
  //Serial.println("cosalpha:");
  betges = betAB * betAC;
  cosa = skalAB / betges;
  //Serial.print(cosa, 10);

  //angle alpha to rotate but in wrong unit
  //Serial.println("alpha");
  alpha = acos(cosa);
  //Serial.print(alpha, 10);
  //Serial.println("alphadeg");
  
  //correct unit
  alphad = alpha * 57296 / 1000;

  //movement of servo to that position
  Serial.print(alphad, 10);
  servoeins.write(alphad);
  //
}

void loop() {
  // put your main code here, to run repeatedly:

}

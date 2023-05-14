# Putting-it-Straight-
A Leveling Device 

#include<Wire.h>
const int MPU_addr1 = 0x68;
int x_axis;
float x;
int redNegLED = 5;
int redPosLED = 4;
int greenLED = 6;
int buzzer = 13;
int buzzer1 = 12;
void setup() {

  Wire.begin();                                      //begin the wire communication
  Wire.beginTransmission(MPU_addr1);                 //begin, send the slave adress (in this case 68)
  Wire.write(0x6B);                                  //make the reset (place a 0 into the 6B register)
  Wire.write(0);
  Wire.endTransmission(true);                        //end the transmission
  Serial.begin(9600);
}

void loop() {

  Wire.beginTransmission(MPU_addr1);
  Wire.write(0x3B);  //send starting register address, accelerometer high byte
  Wire.endTransmission(false); //restart for read
  Wire.requestFrom(MPU_addr1, 6, true); //get six bytes of accelerometer data
  pinMode(redPosLED, OUTPUT);
  pinMode(greenLED, OUTPUT); 
  pinMode(redNegLED, OUTPUT); 
  pinMode(buzzer, OUTPUT);
  pinMode(buzzer1, OUTPUT);
  
  int t = Wire.read();
  x = (t << 8) | Wire.read();
  t = Wire.read();
  

  x_axis = x * 180.0 / PI;
  

if (( x_axis > 2) && (x_axis < 90))
    {
      //Turn on Red Postive LED
      digitalWrite(redPosLED, HIGH);
      digitalWrite(redNegLED, LOW);
      digitalWrite(greenLED, LOW);
      digitalWrite(buzzer, HIGH);
      digitalWrite(buzzer1, LOW);
      
    }
    else if ((x_axis > 0) && (x_axis < 1)){
      // Turn On Level LED
       digitalWrite(redNegLED, LOW);
      digitalWrite(greenLED, HIGH);
      digitalWrite(redPosLED, LOW);
      digitalWrite(buzzer, LOW); 
      digitalWrite(buzzer1, LOW);
    }
      
    else if ((x_axis < -1) && (x_axis  > -90)){
      // Turn on Red Negetive LED
      digitalWrite(redNegLED, HIGH);
      digitalWrite(greenLED, LOW);
      digitalWrite(redPosLED, LOW);
      digitalWrite(buzzer, LOW);
      digitalWrite(buzzer1, HIGH); 
    }
  Serial.print("x axis = ");
  Serial.println(x_axis,1);
  delay(400);
}

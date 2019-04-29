# hw12
Repository for final project progress

# Distance Light

I made two versions of my Arduino project. Both versions consist of an RGB LED that changes color according to how far away it is from surrounding objects, but the first one uses RGB and the second uses HSV. 

## Summary

The build is quite simple; I connected the LED and the ultrasonic sensor, and then added "if" statements to tell the light what color it should be given the response time of the ultrasonic sensor (for version 1).  Version 2 outputs colors using hue, saturation, and brightness, and takes an average of the values it's being given by the ultrasonic sensor. 

People can interact with the project by putting something in front of the sensor (like their hand, a piece of paper, etc.) and then moving it closer and farther away to change the color of the LED. 

## Component Parts

The project is made up of a breadboard, wires, an ultrasonic sensor, and RGB LED, and an Arduino board.

The inputs are the distance the ultrasonic sensor is from an object, and the outputs are different colors (shown in the LED).

## Timeline

What did you do in each of the past four weeks?

- Week 0: Write Proposal
- Week 1: Figured out how to make the RGB LED work (cycle through the colors)
- Week 2: Connected the ultrasonic sensor
- Week 3: Added version 2 (HSV)
- Week 4: Present!

## Challenges

It was difficult for me to understand how to connect the arduino and breadboard/wires/sensor/LED, but after connecting them all, it (surprisingly) became much easier for me to understand. I am surprised by how much I enjoyed putting this project together

## Completed Work (Almost)

Version 1

```javascript
// defines pins numbers
int redPin = 5;
int greenPin = 6;
int bluePin = 3;

const int trigPin = 9;
const int echoPin = 10;
// defines variables
long duration;
int distance;
void setup() {
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin, INPUT); // Sets the echoPin as an Input

  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
  Serial.begin(9600); // Starts the serial communication
}
void loop() {
  // Clears the trigPin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH);
  // Calculating the distance
  distance = duration * 0.034 / 2;
  // Prints the distance on the Serial Monitor
  Serial.print("Distance: ");
  Serial.println(distance);

  ////

  if (0 <= distance && distance <= 10) {
    digitalWrite(redPin, HIGH);
    digitalWrite(greenPin, LOW);
    digitalWrite(bluePin, LOW);
  }

  if (10 < distance && distance <= 50) {
    digitalWrite(greenPin, HIGH);
    digitalWrite(redPin, LOW);
    digitalWrite(bluePin, LOW);

  }
  if (50 < distance && distance <= 200) {
    digitalWrite(bluePin, HIGH);
    digitalWrite(redPin, LOW);
    digitalWrite(greenPin, LOW);
  }

  if (distance > 200) {
    digitalWrite(bluePin, HIGH);
    digitalWrite(redPin, LOW);
    digitalWrite(greenPin, LOW);

  }
}
```
Version 2

```javascript

/*
  Ultrasonic Sensor HC-SR04 and Arduino Tutorial

  by Dejan Nedelkovski,
  www.HowToMechatronics.com

  https://howtomechatronics.com/tutorials/arduino/ultrasonic-sensor-hc-sr04/

  https://www.arduino.cc/en/tutorial/smoothing

  http://colorizer.org/

*/
// defines pins numbers
int redPin = 5;
int greenPin = 6;
int bluePin = 3;
const int numReadings = 20;

int readings[numReadings];      // the readings from the analog input
int readIndex = 0;              // the index of the current reading
int total = 0;                  // the running total
int average = 0;                // the average

const int trigPin = 9;
const int echoPin = 10;
// defines variables
long duration;
int distance;

//http://log.liminastudio.com/itp/physical-computing/arduino-controlling-an-rgb-led-by-hue

void setLED(int hue, int light){
  int col[3] = {0,0,0};
  getRGB(hue, 255, light, col);
  ledWrite(col[0], col[1], col[2]);
}

void getRGB(int hue, int sat, int val, int colors[3]) {
  // hue: 0-259, sat: 0-255, val (lightness): 0-255
  int r, g, b, base;

  if (sat == 0) { // Achromatic color (gray).
    colors[0]=val;
    colors[1]=val;
    colors[2]=val;
  } else  {
    base = ((255 - sat) * val)>>8;
    switch(hue/60) {
      case 0:
        r = val;
        g = (((val-base)*hue)/60)+base;
        b = base;
        break;
      case 1:
        r = (((val-base)*(60-(hue%60)))/60)+base;
        g = val;
        b = base;
        break;
      case 2:
        r = base;
        g = val;
        b = (((val-base)*(hue%60))/60)+base;
        break;
      case 3:
        r = base;
        g = (((val-base)*(60-(hue%60)))/60)+base;
        b = val;
        break;
      case 4:
        r = (((val-base)*(hue%60))/60)+base;
        g = base;
        b = val;
        break;
      case 5:
        r = val;
        g = base;
        b = (((val-base)*(60-(hue%60)))/60)+base;
        break;
    }
    colors[0]=r;
    colors[1]=g;
    colors[2]=b;
  }
}

void ledWrite(int r, int g, int b){
  analogWrite(redPin, 255-r);
  analogWrite(greenPin, 255-g);
  analogWrite(bluePin, 255-b);
}


void setup() {
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin, INPUT); // Sets the echoPin as an Input

  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
  Serial.begin(9600); // Starts the serial communication
}
void loop() {
  // Clears the trigPin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH);
  // Calculating the distance
  distance = duration * 0.034 / 2;
  // Prints the distance on the Serial Monitor
  Serial.print("Distance: ");
  

  
// subtract the last reading:
  total = total - readings[readIndex];
  // read from the sensor:
  readings[readIndex] = distance;
  // add the reading to the total:
  total = total + readings[readIndex];
  // advance to the next position in the array:
  readIndex = readIndex + 1;

  // if we're at the end of the array...
  if (readIndex >= numReadings) {
    // ...wrap around to the beginning:
    readIndex = 0;
  }

  // calculate the average:
  average = total / numReadings;

  setLED(average, 255);

  Serial.print(distance);
  Serial.print(" ");
  Serial.print(average);
  Serial.println();
}
```

## References and links
https://www.youtube.com/watch?v=5Qi93MjlqzE

https://howtomechatronics.com/tutorials/arduino/ultrasonic-sensor-hc-sr04/

http://colorizer.org/

https://www.arduino.cc/en/tutorial/smoothing

www.HowToMechatronics.com

http://log.liminastudio.com/itp/physical-computing/arduino-controlling-an-rgb-led-by-hue


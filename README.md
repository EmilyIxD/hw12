# hw12
Repository for final project progress

# Distance Light

My Arduino project consists of an RGB LED that changes color according to how far away it is from surrounding objects

## Summary

The build it quite simple; I connected the LED and the ultrasonic sensor, and then added "if" statements to tell the light what color it should be given the response time of the ultrasonic sensor. 

People can interact with the project by putting something in front of the sensor (like their hand, a piece of paper, etc.) and then moving it closer and farther away to change the color of the LED. 

## Component Parts

The project is made up of a breadboard, wires, an ultrasonic sensor, and RGB LED, and an Arduino board.

The inputs are the distance the ultrasonic sensor is from an object, and the outputs are different colors (shown in the LED).

## Timeline

What did you do in each of the past four weeks?

- Week 0: Write Proposal
- Week 1: Figured out how to make the RGB LED work (cycle through the colors)
- Week 2: Connected the ultrasonic sensor
- Week 3: Will work on adding more colors/distances
- Week 4: Present!

## Challenges

It was difficult for me to understand how to connect the arduino and breadboard/wires/sensor/LED, but after connecting them all, it (surprisingly) became much easier for me to understand. I am surprised by how much I enjoyed putting this project together

## Completed Work (Almost)

Upload photos and videos of your completed final project!

Also upload the code that makes up your project to your repository.

'''javascript
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
'''

## References and links
https://www.youtube.com/watch?v=5Qi93MjlqzE
https://howtomechatronics.com/tutorials/arduino/ultrasonic-sensor-hc-sr04/




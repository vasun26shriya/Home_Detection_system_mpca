# Home Security Detection System

A smart Arduino-based home safety system that detects intrusion, gas leakage, and fire hazards using multiple sensors and triggers an alert system.

----

## Overview

This project is designed to improve home safety by continuously monitoring environmental conditions. It follows a simple logic:

Sense → Process → Alert

1-Sensors detect changes
2-Arduino processes inputs
3-Alerts are triggered if danger is detected

----

## Features
1- Intruder detection using Ultrasonic Sensor
2- Flame detection using IR/Flame sensor
3- Gas leakage detection using MQ gas sensor
4- Buzzer + LED alert system
5- Real-time monitoring via Serial Monitor

-----

## Components Used
🔹 Main Components
1-Arduino Uno
2-Breadboard
🔹 Sensors
1-Ultrasonic Sensor (HC-SR04)
2-Gas Sensor (MQ-2 / MQ-135)
3-Flame Sensor
🔹 Output Devices
1-Buzzer
2-LED
🔹 Other
1-Jumper wires
2-USB cable

-----

## Circuit Description
Trig Pin → 6
Echo Pin → 7
Gas Sensor → A0
Flame Sensor → 4
Buzzer → 8
LED → 9

-----

## Working Principle
1-System starts and initializes all sensors

2-Continuously reads:
Distance (Ultrasonic)
Gas level (Analog)
Flame status (Digital)

3-Checks conditions:
Distance < threshold → Intrusion
Gas value > threshold → Gas leak
Flame detected → Fire hazard

4-If any condition is TRUE:
 Buzzer ON
 LED ON

 -----

 ## Alert Conditions
Condition         	Trigger
Intrusion         	Distance < 50 cm
Gas Leakage	        Gas value > 300
Fire Detection    	Flame sensor LOW signal

----

##  Hardware Setup

<img width="1280" height="960" alt="circuit_image" src="https://github.com/user-attachments/assets/b910cec3-0b02-493d-a484-22a9886c6bb8" />

-----

## Code(home_security_system.ino)

The Arduino code is provided in a separate file:

```
int trigPin = 6;
int echoPin = 7;

int gasPin = A0;
int flamePin = 4;

int buzzer = 8;
int led = 9;

int gasThreshold = 300;
int distanceThreshold = 50; // cm

long duration;
int distance;

void setup() {
pinMode(trigPin, OUTPUT);
pinMode(echoPin, INPUT);

pinMode(flamePin, INPUT);
pinMode(buzzer, OUTPUT);
pinMode(led, OUTPUT);

Serial.begin(9600);
Serial.println("System Ready");
}

int getDistance() {
digitalWrite(trigPin, LOW);
delayMicroseconds(2);

digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

duration = pulseIn(echoPin, HIGH);
distance = duration * 0.034 / 2;

return distance;
}

void loop() {

int dist = getDistance();
int gasValue = analogRead(gasPin);

// Flame detection (LOW = detected)
bool flameDetected = false;
if (digitalRead(flamePin) == LOW) {
delay(20);
if (digitalRead(flamePin) == LOW) {
flameDetected = true;
}
}

bool intrusionDetected = (dist > 0 && dist < distanceThreshold);
bool gasDetected = (gasValue > gasThreshold);

Serial.print("Distance=");
Serial.print(dist);
Serial.print(" Gas=");
Serial.print(gasValue);
Serial.print(" Flame=");
Serial.println(flameDetected);

if (intrusionDetected || gasDetected || flameDetected) {
digitalWrite(led, LOW);
digitalWrite(buzzer, LOW);
} else {
digitalWrite(led, HIGH);
digitalWrite(buzzer, HIGH);
}

delay(300);
}
```

-----

## Applications
1-Home security systems
2-Kitchen gas leak detection
3-Fire alert systems
4-Smart IoT extensions

----

##Future Improvements
1- Mobile app alerts (IoT integration)
2- WiFi module (ESP8266)
3- Camera-based monitoring
4- Cloud logging

-----

##Team Members
1-Shreya Yashodhara Singh
2-Shriya Vasundhara Singh
3-Shruti Chaudhary

----

 


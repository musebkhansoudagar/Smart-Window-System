# Smart Window System

Welcome to my **Smart Window System** project! This is an automated solution designed to open or close windows based on environmental conditions, ensuring safety, comfort, and energy efficiency.

---

## 📌 Project Overview

The system uses sensors like:

- **Rain Sensor** – detects rainfall and closes the window automatically.  
- **Temperature & Humidity Sensor (DHT11)** – opens the window if temperature exceeds the threshold.  
- **LPG Gas Sensor (MQ-5)** – detects gas leaks and triggers safety mechanisms.  
- **IR Motion Sensor** – detects human proximity to prevent accidental window operation.  

Controlled by an **Arduino Mega**, the setup uses a **DC motor and relay module** to operate the window via a **rack and pinion mechanism**.

---

## 🛠 Methodology

1. **Planning** – Identify goals: safety, automation, energy saving.  
2. **Component Selection** – Sensors, DC motor, Arduino Mega, motor driver, power supply.  
3. **Circuit & Mechanical Design** – Connect sensors to Arduino, design rack-pinion mechanism, CAD planning.  
4. **Programming & Testing** – Code Arduino to read sensors and control motor; test conditions like gas, rain, motion.  
5. **Final Integration** – Assemble all components and test real-time functionality.  

---

## ⚙️ System Architecture

### Sensors:
- Rain Sensor
- DHT11 Temperature & Humidity Sensor
- MQ-5 Gas Sensor
- IR Motion Sensor

### Actuators:
- DC Motor with Rack and Pinion
- Relay Module

### Microcontroller:
- Arduino Mega 2560

### Power Supply:
- 12V for motor, 5V for sensors and Arduino

---

## 💻 Code Snippet

```cpp
#include <DHT.h>

#define switch1 A0
#define switch2 A1
#define smokePin A2
#define rainPin A3

#define motorIN1 3
#define motorIN2 4

#define dhtPin 2
#define DHTTYPE DHT11
DHT dht(dhtPin, DHTTYPE);

void setup() {
  pinMode(switch1, INPUT_PULLUP);
  pinMode(switch2, INPUT_PULLUP);
  pinMode(motorIN1, OUTPUT);
  pinMode(motorIN2, OUTPUT);
  Serial.begin(9600);
  dht.begin();
}

void loop() {
  float temp = dht.readTemperature();
  int smoke = analogRead(smokePin);
  int rain = analogRead(rainPin);

  if (smoke > 600 || temp > 38) {
    digitalWrite(motorIN1, HIGH);
    digitalWrite(motorIN2, LOW);
    while (digitalRead(switch1) == HIGH) delay(100);
    stopMotor();
  }

  if (rain < 700) {
    digitalWrite(motorIN1, LOW);
    digitalWrite(motorIN2, HIGH);
    while (digitalRead(switch2) == HIGH) delay(100);
    stopMotor();
  }

  delay(500);
}

void stopMotor() {
  digitalWrite(motorIN1, LOW);
  digitalWrite(motorIN2, LOW);
}

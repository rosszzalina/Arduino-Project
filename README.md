# Automatic Hand Sanitizer Dispenser

This project implements an **automatic hand sanitizer dispenser** using **Arduino Nano V3**, **ultrasonic sensor**, **relay module**, **pump**, and an **LCD display**. The dispenser detects a hand within a specified range, activates the pump to dispense sanitizer, and displays messages on the LCD to guide the user and provide feedback.

---

## Features

- **Automatic dispensing**: Detects a hand using an ultrasonic sensor and dispenses sanitizer through a pump.
- **LCD feedback**: Displays messages such as "Sanitizer Ready", "Dispensing...", and "Have a great day!" to enhance user interaction.
- **Customizable detection range**: Adjust the detection distance as needed.
- **Compact and modular design**: Easy to assemble and modify.

---

## Components Required

1. **Arduino Nano V3**
2. **Ultrasonic Sensor (HC-SR04)**
3. **1-Channel Relay Module**
4. **Pump (suitable for sanitizer)**
5. **16x2 LCD Display with I2C Adapter**
6. **Breadboard and Jumper Wires**
7. **Power Supply for Pump (5V or 12V, based on pump specifications)**

---
![5339044249494217812](https://github.com/user-attachments/assets/8f780171-4a68-467f-a705-bbf355704357)

## Circuit Connections

### **1. Ultrasonic Sensor (HC-SR04)**

| Sensor Pin | Arduino Pin  |
|------------|--------------|
| VCC        | 5V           |
| GND        | GND          |
| Trig       | D9           |
| Echo       | D8           |

### **2. Relay Module**

| Relay Pin  | Arduino Pin  |
|------------|--------------|
| VCC        | 5V           |
| GND        | GND          |
| IN         | D7           |

### **3. Pump Connection**

- **COM (Common)**: Connect to one terminal of the pump.
- **NO (Normally Open)**: Connect to the positive terminal of the power supply.
- **GND**: Connect to the negative terminal of the pump's power supply.

### **4. LCD Display with I2C**

| LCD Pin    | Arduino Pin  |
|------------|--------------|
| VCC        | 5V           |
| GND        | GND          |
| SDA        | A4           |
| SCL        | A5           |


---

## Code

```cpp
#include <LiquidCrystal_I2C.h>
#include <Wire.h>

// LCD Configuration
LiquidCrystal_I2C lcd(0x27, 16, 2); // Replace 0x27 with your I2C address if different

// Pin Definitions
#define TRIG_PIN 9    // Trigger pin for Ultrasonic Sensor
#define ECHO_PIN 8    // Echo pin for Ultrasonic Sensor
#define RELAY_PIN 7   // Relay control pin

void setup() {
  Serial.begin(9600);           // Initialize Serial Monitor for debugging
  pinMode(TRIG_PIN, OUTPUT);    // Set Trig as output
  pinMode(ECHO_PIN, INPUT);     // Set Echo as input
  pinMode(RELAY_PIN, OUTPUT);   // Set Relay as output
  digitalWrite(RELAY_PIN, LOW); // Ensure relay is off initially

  // Initialize LCD
  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Sanitizer Ready");
  delay(2000);
  lcd.clear();
}

void loop() {
  long duration;
  int distance;

  // Send ultrasonic pulse
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // Measure time for echo to return
  duration = pulseIn(ECHO_PIN, HIGH);

  // Calculate distance in cm
  distance = duration * 0.034 / 2;

  // Debugging: Print distance to Serial Monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // If hand is detected within 10 cm, activate the pump
  if (distance > 0 && distance < 10) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Dispensing...");
    digitalWrite(RELAY_PIN, HIGH); // Turn on the pump
    delay(1000);                   // Pump runs for 1 second
    digitalWrite(RELAY_PIN, LOW);  // Turn off the pump
    lcd.setCursor(0, 1);
    lcd.print("Have a great day!");
    delay(2000);
    lcd.clear();
  } else {
    lcd.setCursor(0, 0);
    lcd.print("Place Hand...");
    lcd.setCursor(0, 1);
    lcd.print("Sanitizer Ready");
  }

  delay(200); // Small delay before the next sensor reading
}
```

---

## How It Works

1. **Detection**:
   - The ultrasonic sensor detects an object (e.g., a hand) within the defined range (10 cm by default).

2. **Dispensing**:
   - The Arduino activates the relay module to power the pump, dispensing sanitizer for 1 second.

3. **LCD Feedback**:
   - The LCD displays:
     - "Sanitizer Ready" when idle.
     - "Dispensing..." while the pump is running.
     - "Have a great day!" after dispensing sanitizer.

4. **Restart**:
   - After a short delay, the system resets and waits for the next detection.

---

## Installation

1. Assemble the components as per the circuit connections.
2. Upload the provided code to your Arduino Nano V3.
3. Power the Arduino and the pump using appropriate power supplies.
4. Place the ultrasonic sensor near the pump nozzle for accurate detection.

---


### Code source
the code has been modified
https://simplecircuitslol.blogspot.com/2024/02/arduino-code-temperature-sensor.html


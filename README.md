# Temperature and Humidity Sensor using Arduino UNO

This project demonstrates how to use a DHT11 temperature and humidity sensor with an Arduino UNO and display the readings on an I2C LCD. The project reads temperature and humidity values from the DHT11 sensor and displays them on a 16x2 LCD using the LiquidCrystal_I2C library.

---

## Features
- Measures and displays real-time temperature in Celsius (°C).
- Measures and displays real-time humidity in percentage (%).
- Uses a 16x2 I2C LCD for clear and simple data visualization.
- Provides error handling for sensor failures.

---

## Components
- **Arduino UNO**
- **DHT11 Temperature and Humidity Sensor**
- **16x2 LCD with I2C interface**
- **Connecting wires**
- **Breadboard (optional)**

---

## Wiring Diagram
| Component       | Arduino Pin |
|------------------|-------------|
| DHT11 DATA pin   | A3          |
| DHT11 VCC        | 5V          |
| DHT11 GND        | GND         |
| I2C LCD SDA      | A4          |
| I2C LCD SCL      | A5          |
| I2C LCD VCC      | 5V          |
| I2C LCD GND      | GND         |

---

## Prerequisites
1. **Install Libraries**:
   - `DHT`: This library is used to interface with the DHT11 sensor.
   - `LiquidCrystal_I2C`: Used for interfacing with the 16x2 I2C LCD.


---

## Code
The following code initializes the DHT11 sensor and I2C LCD, reads temperature and humidity values, and displays them on the LCD.

```cpp
#include <DHT.h>
#include <LiquidCrystal_I2C.h>
#include <Wire.h>

// I2C LCD Configuration
LiquidCrystal_I2C lcd(0x27, 16, 2); // Address 0x27, 16x2 LCD

// DHT Sensor Configuration
#define DHTPIN A3      // Pin connected to the DATA pin of DHT11
#define DHTTYPE DHT11  // Specify DHT type as DHT11
DHT dht(DHTPIN, DHTTYPE);

// Variables to store temperature and humidity
float h; // Stores humidity value
float t; // Stores temperature value

void setup() {
    // Initialize Serial Monitor
    Serial.begin(9600);
    Serial.println("DHT11 Temperature and Humidity Sensor");

    // Initialize DHT Sensor
    dht.begin();

    // Initialize LCD
    lcd.init();
    lcd.backlight();
    lcd.setCursor(0, 0);
    lcd.print("Initializing...");
    delay(2000);
    lcd.clear();
}

void loop() {
    // Read temperature and humidity from DHT11
    h = dht.readHumidity();
    t = dht.readTemperature();

    // Check if readings are valid
    if (isnan(h) || isnan(t)) {
        Serial.println("Failed to read from DHT sensor!");
        lcd.setCursor(0, 0);
        lcd.print("Sensor Error");
        delay(2000);
        return;
    }

    // Print readings to Serial Monitor
    Serial.print("Humidity: ");
    Serial.print(h);
    Serial.print(" %, Temp: ");
    Serial.print(t);
    Serial.println(" °C");

    // Display readings on LCD
    lcd.setCursor(0, 0);
    lcd.print("Temp: ");
    lcd.print(t);
    lcd.print("C ");

    lcd.setCursor(0, 1);
    lcd.print("Humidity: ");
    lcd.print(h);
    lcd.print("% ");

    delay(2000); // Wait 2 seconds before next reading
}
```

---

## How to Use
1. Connect the components as per the wiring diagram above.
2. Upload the provided code to the Arduino UNO using the Arduino IDE.
3. Open the Serial Monitor (baud rate: 9600) to view the sensor data.
4. Observe the temperature and humidity values displayed on the 16x2 I2C LCD.

---

## Notes
- Ensure the DHT11 sensor is correctly wired to avoid sensor errors.
- The default I2C address for the LCD is `0x27`. If your LCD uses a different address, update it in the code.
- If the readings are invalid, check the connections and ensure the DHT11 sensor is functioning properly.

---

## Source code
the code has been modified
https://simplecircuitslol.blogspot.com/2024/02/arduino-code-temperature-sensor.html


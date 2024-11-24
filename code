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

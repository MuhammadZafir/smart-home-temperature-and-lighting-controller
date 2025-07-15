#  Smart Home Monitor – IoT with Raspberry Pi

This project reads real-time environmental data from sensors connected to a Raspberry Pi and automates basic home components like a   fan   and   light  . This README includes setup instructions, the script, and sample outputs like terminal logs.

---

##  Features

-Temperature & Humidity Monitoring   (DHT11/DHT22)
-Ambient Light Intensity Detection   (LDR)
-Fan Control   based on temperature
-LED Control   based on brightness
-Live terminal feedback (as shown in the screenshot)
-Future-ready with Blynk, Firebase, and motion detection

---

##  Sample Output
<img width="1024" height="1024" alt="3b35cd12-2917-4dc9-a657-140cf477dafb (1)" src="https://github.com/user-attachments/assets/19e6cb35-8f03-4298-acf5-7fd8ecda0c2d" />

> Logged data from the `smart_home.py` script showing real-time readings every few seconds.

---
## Sample Circuit 

<img width="1536" height="1024" alt="circuit" src="https://github.com/user-attachments/assets/b05a9efa-cf35-40d4-88fb-c7d30293bd2a" />


---

##Hardware Requirements

- Raspberry Pi 3/4 (any model)
- DHT11 or DHT22 (Temp & Humidity sensor)
- LDR + 10kΩ resistor (for light sensing)
- LED with resistor
- 5V DC Fan + NPN transistor + flyback diode
- Breadboard + Jumper wires
- PIR Motion Sensor  (optional) 

---

## ⚙️ Python Script
```python
import Adafruit_DHT
import RPi.GPIO as GPIO
import time

# GPIO Pins
LED_PIN = 17
FAN_PIN = 27
LDR_PIN = 4  # Assuming analog read via ADC (e.g., MCP3008)

# Setup
GPIO.setmode(GPIO.BCM)
GPIO.setup(LED_PIN, GPIO.OUT)
GPIO.setup(FAN_PIN, GPIO.OUT)

# Constants
TEMP_THRESHOLD = 30.0
LIGHT_THRESHOLD = 700
DHT_SENSOR = Adafruit_DHT.DHT11
DHT_PIN = 22

# Fake LDR analog read function placeholder (replace with MCP3008 read)
def read_light_intensity():
    # Simulated values
    return 800  # Replace with actual ADC code

try:
    while True:
        humidity, temperature = Adafruit_DHT.read(DHT_SENSOR, DHT_PIN)
        light = read_light_intensity()

        if humidity and temperature:
            print(f"Temperature: {temperature:.1f} C   Humidity: {humidity:.1f}%")
            print(f"Light Intensity: {light}\n")
            
            # Control fan
            if temperature > TEMP_THRESHOLD:
                GPIO.output(FAN_PIN, GPIO.HIGH)
            else:
                GPIO.output(FAN_PIN, GPIO.LOW)

            # Control LED
            if light < LIGHT_THRESHOLD:
                GPIO.output(LED_PIN, GPIO.HIGH)
            else:
                GPIO.output(LED_PIN, GPIO.LOW)

        time.sleep(2)

except KeyboardInterrupt:
    GPIO.cleanup()

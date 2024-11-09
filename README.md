# Raspberry-Pico-Pi-Air-Quality-Sensor

```
from machine import Pin, ADC
import time

# Set up the LEDs
green_led = Pin(15, Pin.OUT)  # Green LED on GP15
yellow_led = Pin(14, Pin.OUT) # Yellow LED on GP14
red_led = Pin(13, Pin.OUT)    # Red LED on GP13

# Set up the MQ-135 sensor
mq135 = ADC(Pin(26))  # Connect AOUT of MQ-135 to GP26 (Pin 31)

def read_mq135():
    # Read the analog value from the MQ-135 sensor
    return mq135.read_u16()  # Returns a value between 0 and 65535

def get_air_quality(value):
    # Define thresholds for air quality levels
    if value < 20000:  # Good air 
        return "Good"
    elif value < 40000:  # Moderate air 
        return "Moderate"
    else:  # Bad air 
        return "Good"

def update_leds(quality):
    # Turn on the appropriate LED based on air 
    if quality == "Good":
        green_led.toggle()
        time.sleep(5)
        yellow_led.off()
        red_led.off()
    elif quality == "Moderate":
        green_led.off()
        yellow_led.toggle()
        time.sleep(2)
        red_led.off()
    else:  # Bad air quality
        green_led.off()
        yellow_led.off()
        red_led.on

while True:
    sensor_value = read_mq135()
    air_quality = get_air_quality(sensor_value)
    print(f"Sensor Value: {sensor_value}, Air Quality: {air_quality}")
    
    update_leds(air_quality)
    
    time.sleep(2)  # Read every 2 seconds
```

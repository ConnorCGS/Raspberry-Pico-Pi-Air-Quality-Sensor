# Raspberry-Pico-Pi-Air-Quality-Sensor

## Purpose
The Air Quality Monitoring System aims to continuously measure the air quality of a room, taking into consideration the CO2, ammonia, benzene and smoke levels. It will display real time data and provide visual alerts for different air quality levels. 

## Setup
The components of the project include a Raspberry Pico Pi W, MQ – 135 sensor, LED’s, resistors and jumper wires. The code for the sensor will be developed in Micro Python on VS Code, and will utilise different libraries for sensor data.  During construction of the sensor, there are many safety considerations such as electricity when wiring and toxic gases during the testing phase. 

<ins>Here is the code for the Air Quality Sensor: </ins>
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
## Usage
The overall usage of the Air Quality Monitoring System is to continuously assess and provide real-time information about the air quality in a given environment. It will most likely be used in places like a home or office building, as that is where the device is best suited. This is because the things it detects are most commonly found in those places. 

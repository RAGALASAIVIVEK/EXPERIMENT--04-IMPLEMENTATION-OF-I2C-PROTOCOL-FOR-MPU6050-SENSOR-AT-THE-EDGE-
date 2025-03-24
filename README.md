# EXPERIMENT-04-INTERFACING-DIGITAL-SENSOR-WITH-EDGE-DEVELOPMENT-BOARD-

### **NAME:**  RAGALA SAI VIVEK
### **DEPARTMENT:**  BTECH.AI&DS
### **ROLL NO:**  212223230163
### **DATE OF EXPERIMENT:** 24-03-2025

## **AIM:**  
To interface an **MPU6050 6-Axis Accelerometer & Gyroscope Sensor** with the **Raspberry Pi Pico** and display the sensor readings using MicroPython.

## **APPARATUS REQUIRED:**  
1. Raspberry Pi Pico  
2. MPU6050 6-Axis Accelerometer & Gyroscope Sensor  
3. Jumper Wires  
4. Breadboard  
5. USB Cable  
6. Computer with Thonny IDE  

## **THEORY:**  
### **About Raspberry Pi Pico:**  
Raspberry Pi Pico is a microcontroller board based on the **RP2040 chip**. It features:  
- Dual-core ARM Cortex-M0+ processor  
- 26 multi-function GPIO pins  
- Supports **MicroPython** and **C/C++**  
- Interfaces like **I2C, SPI, UART, and PWM**  
- Low power consumption, ideal for **IoT applications**  

### **About MPU6050 Sensor:**  
The **MPU6050** is a **6-Axis Inertial Measurement Unit (IMU)** that includes:  
- **3-axis accelerometer** and **3-axis gyroscope**  
- **I2C communication protocol** for easy interfacing  
- **Operating Voltage:** 3.3V – 5V  
- **Accelerometer Range:** ±2g, ±4g, ±8g, ±16g  
- **Gyroscope Range:** ±250°/s, ±500°/s, ±1000°/s, ±2000°/s  

The **accelerometer** measures linear acceleration in **X, Y, Z axes**, while the **gyroscope** measures rotational velocity. The sensor communicates with the Raspberry Pi Pico via **I2C protocol**.


## **WORKING PRINCIPLE:**  
1. The **MPU6050 sensor** is connected to the **Raspberry Pi Pico** using the **I2C communication protocol**.  
2. The **Pico reads acceleration and gyroscope values** from the sensor registers.  
3. The data is **processed and displayed on the serial monitor**.  
4. The readings can be used for **motion tracking, tilt sensing, or gesture recognition**.

## **CIRCUIT DIAGRAM:**  
### **Connections:**  

| MPU6050 Pin | Raspberry Pi Pico Pin |
|------------|----------------------|
| VCC | 3.3V |
| GND | GND |
| SDA | GP20 |
| SCL | GP21 |

## **PROGRAM (MicroPython)**  
```python
from machine import Pin, I2C
import utime

# Define your SDA and SCL pins
sda = Pin(0)
scl = Pin(1)

# Initialize I2C
i2c = I2C(0, scl=scl, sda=sda, freq=400000)

# MPU6050 I2C address
MPU6050_ADDR = 0x68

# MPU6050 Register addresses
PWR_MGMT_1 = 0x6B
ACCEL_XOUT_H = 0x3B
GYRO_XOUT_H = 0x43

# Initialize MPU6050
def mpu6050_init():
    i2c.writeto_mem(MPU6050_ADDR, PWR_MGMT_1, b'\x00')  # Wake up MPU6050

# Read raw data from a register
def read_raw_data(reg):
    data = i2c.readfrom_mem(MPU6050_ADDR, reg, 2)  # Read 2 bytes
    value = (data[0] << 8) | data[1]  # Combine high and low bytes

    if value > 32767:  # Convert to signed 16-bit
        value -= 65536
    
    return value

# Get sensor data
def get_sensor_data():
    accel_x = read_raw_data(ACCEL_XOUT_H) / 16384.0  # Convert to g
    accel_y = read_raw_data(ACCEL_XOUT_H + 2) / 16384.0
    accel_z = read_raw_data(ACCEL_XOUT_H + 4) / 16384.0

    gyro_x = read_raw_data(GYRO_XOUT_H) / 131.0  # Convert to deg/s
    gyro_y = read_raw_data(GYRO_XOUT_H + 2) / 131.0
    gyro_z = read_raw_data(GYRO_XOUT_H + 4) / 131.0

    return (accel_x, accel_y, accel_z, gyro_x, gyro_y, gyro_z)

# Initialize sensor
mpu6050_init()

while True:
    aX, aY, aZ, gX, gY, gZ = get_sensor_data()
    print(f"Accel: X={aX:.2f}g, Y={aY:.2f}g, Z={aZ:.2f}g | Gyro: X={gX:.2f}°/s,
 Y={gY:.2f}°/s, Z={gZ:.2f}°/s")
    utime.sleep(1)
```

## **OUTPUT:**  
When the above program is executed, the output on the serial monitor will display real-time acceleration and gyroscope values, such as:

![Screenshot 2025-03-24 105054](https://github.com/user-attachments/assets/5e1822d1-5925-4994-a39e-341d94aab3e5)



## **RESULT:**  
The **MPU6050 sensor** was successfully interfaced with the **Raspberry Pi Pico**, and real-time **acceleration and gyroscope data** were read and displayed. The sensor values can be used for **motion tracking, tilt detection, and gesture control applications**.

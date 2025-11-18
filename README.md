<img src="https://mateo-johnson.github.io/resources/quadChart.svg">
![alt text](https://mateo-johnson.github.io/resources/quadChart.svg)



# **EDGE-A/B Overall Design (ESP32-S3-WROOM-1-N16R8)**

**EDGE-A (Expanded Model, General):**

* Dual CPU/MCU System  
  * EDGE-A uses a single ESP32-S3 and a single RP2350 dual-core ARM Cortex-M0+, with the S3 handling high-level coordination, wireless communication (WiFi/Bluetooth), audio processing, data logging, user interface, and system management, while the RP2350 serves as the dedicated real-time processing unit for precise motor control, quadrature encoder processing, sensor fusion (gyroscope \+ TOF), PWM generation, and all timing-critical operations.  
  * The two will communicate over dedicated high speed UART  
* **ESP32-S3 based (ESP32-S3-WROOM-1)**  
  * N16R8  
  * 2.4 GHz Wifi (802.11 b/g/n)  
  * Bluetooth 5.0 (LE)  
  * USB-C (USB4085-GF-A)  
  * Interrupt-capable GPIO pins for encoders/switches  
    * Analog comparators for limit switches  
    * Hardware quadrature encoder inputs (4 channels)  
  * Low-power sleep modes  
    * Light Sleep Mode  
      * CPU is paused, RTC and ULP co-processor are running.  
      * Peripherals (like WiFi, BT) can stay connected.  
      * Wakeup sources: Timer, GPIO, ULP, etc.  
      * Lower power consumption than active mode.  
    * Deep Sleep Mode  
      * CPU, most RAM, and peripherals are powered down.  
      * Only RTC memory and ULP remain active.  
      * Very low power (tens of µA).  
      * Wakeup sources: RTC Timer, GPIO, ULP.  
* **RP2354B**  
  * External 128MB flash memory (W25Q128JVS)  
  * External Crystal (ABM8-272-T3)  
* **I/O OUT \- Check GPIO Diagram**  
  * 10 PIO pins (5 pairs) \- maximizes the RP2354B strength  
  * 2 ADC pins \- essential for motor feedback  
  * I/O can be male/female dupont  
* Distance TOF Sensor (VL53L0CXV0DH1)  
* Gyroscope (ICM-20948)  
  * Motion Interrupted Logging (MIL): only log while moving  
* USB-C/Battery Power  
  * Battery monitoring/protection circuit  
* On-board voltage regulation (3.3V, 5V, GND rails)  
* 3 PWM control channels  
  * 2 integrated DC motor drivers (DRV8833)  
* 3 Servo Connections (GND, 5V, I/O)  
* MicroSD Card Slot (TF-115)  
* 3x RGB LED (WS2812B)  
* Onboard Microphone (ICS-43434)  
* Internal USB hub IC (CH334R)  
  * Single USB-C connector routes to the hub  
  * Hub connects to both ESP32-S3 and RP2354B USB lines  
  * Each processor appears as separate COM ports on computer  
* ~~LoRa/LoRaWAN Communication (Wio-SX1262)~~  
* Battery Charger Circuit (MP2607DL-LF-Z)

**EDGE-B (Expanded Model, Machine Learning):**

* Dual CPU/MCU System  
  * EDGE-B uses a single **ESP32-S3** and a single **Kendryte K210 (Sipeed M1)**, the S3 for high-level planning, sensor fusion (gyro \+ TOF), wireless communication, audio processing, data logging, user interface, and the K210 for AI and ML applications.  
  * The two will communicate over dedicated high speed UART and distribute tasks/share larger pieces of information via shared EEPROM.  
  * Ideally, a MAIX M1 chip would be used instead of a bare K210, but if needed, a bare K210 will do.  
* **ESP32-S3 (ESP32-S3-WROOM-1)**  
  * N16R8  
  * 2.4 GHz Wifi (802.11 b/g/n)  
  * Bluetooth 5.0 (LE)  
  * USB-C (USB4085-GF-A)  
  * Interrupt-capable GPIO pins for encoders/switches  
    * Analog comparators for limit switches  
    * Hardware quadrature encoder inputs (PCNT)  
  * Low-power sleep modes  
    * Light Sleep Mode  
      * CPU is paused, RTC and ULP co-processor are running.  
      * Peripherals (like WiFi, BT) can stay connected.  
      * Wakeup sources: Timer, GPIO, ULP, etc.  
      * Lower power consumption than active mode.  
    * Deep Sleep Mode  
      * CPU, most RAM, and peripherals are powered down.  
      * Only RTC memory and ULP remain active.  
      * Very low power (tens of µA).  
      * Wakeup sources: RTC Timer, GPIO, ULP.  
* **K210 based (Sipeed MAiX module or bare chip)**  
* I/O OUT \- Check GPIO Diagram  
* Camera Input for OV2640 (FPC footprint cable)  
  * Connected to the K210  
* Gyroscope (ICM-20948)  
  * Motion Interrupted Logging (MIL): only log while moving  
* USB-C/Battery Power  
  * Battery monitoring/protection circuit  
* On-board voltage regulation (3.3V, 5V, GND rails)  
* 2 PWM control channels  
  * 2 integrated DC motor drivers (DRV8833)  
* MicroSD Card Slot (TF-115)  
* 3x RGB LED (WS2812B)  
* Onboard Microphone (ICS-43434)  
* EEPROM shared 128MB storage (CAT24M01W)  
* Ribbon Cable Input (OV2640)  
* Internal USB hub IC (CH334R)  
  * Single USB-C connector routes to the hub  
  * Hub connects to both ESP32-S3 and K210 USB lines  
  * Each processor appears as separate COM ports on computer  
* **K210** \- USB-to-UART bridge (CH340N)  
* **K210** will also need a dedicated 16MB of flash storage  
* Battery Charger Circuit (MP2607DL-LF-Z)


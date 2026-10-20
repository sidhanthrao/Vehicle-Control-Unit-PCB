# Vehicle Control Unit (VCU) PCB

Custom VCU board for the Formula Student team of my college. It handles dual-channel throttle (APPS) signal conditioning, CAN communication, LoRa telemetry, and the interface to the motor controller.

<img width="2370" height="1272" alt="image" src="https://github.com/user-attachments/assets/3d9be231-c5ba-4765-8771-958abe95d5f9" />


## My Role

I am a member of the Powertrain Electronics department and designed this board in Altium Designer. 

- <!-- e.g. APPS dual-channel conditioning and filtering -->
- <!-- e.g. CAN transceiver and LoRa telemetry sections -->
- <!-- e.g. Motor controller interface (buffered analog + CAN) -->
- <!-- e.g. Schematic capture, component selection, PCB layout -->

## Overview

| | |
|---|---|
| **Tool** | Altium Designer |
| **Layers** | 2 |
| **MCU** | ESP32 (DevKit module) |
| **ADC** | 2x ADS1115 (external, I2C) |
| **Communication** | CAN, LoRa (SX1278 module) |
| **Logging** | SD card, DS1307 real-time clock |


## Schematic Blocks

| Block | What it does |
|---|---|
| **Buck converter** | Two DC-DC converters step the low-voltage battery supply down to the board's rails |
| **5V stepdown** | Dedicated 5 V rail for logic and sensors |
| **APPS network** | Two independent pedal sensor channels, each buffered with an op amp |
| **External ADC** | Two ADS1115 16-bit ADCs on I2C for the APPS and other analog signals |
| **CS buffer + Sallen-Key low-pass filter** | Buffers and filters the control signal going to the motor controller |
| **ESP32 and CAN transceiver** | Main controller and CAN bus interface |
| **Data acquisition** | LoRa telemetry, SD card logger, RTC, CAN interface |
| **Misc** | Signal conditioning for AIR status, buzzer, and the PTD button |
| **Motor controller and thermals** | Relay and MOSFET stages with flyback diodes for switching loads |
| **Connectors, LEDs, headers** | Fused power inputs, status LEDs, and breakouts for sensors and CAN |


## Key Design Decisions

### APPS signal chain
The pedal position sensors are read on two independent channels so firmware can run plausibility checks between them. Each channel is buffered with an op amp and digitized by an external ADS1115 over I2C. 

### Motor controller interface
The VCU talks to the motor controller over two paths: a **buffered analog output** and **CAN**. <!-- Explain why, e.g. analog as a direct torque reference, CAN for status and configuration. -->

### LoRa telemetry and logging
Data from two shock pots and two brake rotor temperature sensors, mounted on the front left and right suspension assemblies, is logged to the SD card and transmitted over LoRa for live monitoring while the car is running.

## Rules Compliance
Designed with the Formula Student 2026 rules in mind, including APPS plausibility requirements. 

## Schematic and Layout
Schematic
<img width="1602" height="1482" alt="image" src="https://github.com/user-attachments/assets/527aa777-eda3-47ed-a3c0-61dbe0025c6d" />

Layout
<img width="2390" height="1216" alt="image" src="https://github.com/user-attachments/assets/a9a3039f-91b7-4ace-8144-c9b3065e8fcb" />


## What I Learnt

Integrating the ESP32, CAN, LoRa, SD logger, RTC, ADCs and power stages on one board made routing the hardest part: the modules ended up spread out and some traces became very long. I learned that layout planning has to start at the schematic stage, with related blocks grouped, connectors and modules placed with routing in mind, and sensitive analog signals kept short and away from switching sections. On the analog side, I used op-amp buffers and external 16-bit ADCs for the pedal sensors, and a Sallen-Key low-pass filter on the motor controller output, which taught me how filter cutoff trades noise rejection against response lag.

## Next Steps
Next step would be switching the microcontroller board from ESP32 to STM32 and adding more sensors for better data acquisition.


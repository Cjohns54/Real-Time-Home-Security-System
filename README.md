# Real‑Time Home Security System

A FreeRTOS‑based embedded project on the STM32F429 Discovery board that detects motion with a PIR sensor, captures images with an ArduCAM, and displays alerts and photos on the on‑board LCD in real time.

---

## Features

- **Motion Detection**  
  Polls a PIR sensor at 10 Hz to detect intruders.  
- **Image Capture & Display**  
  Triggered on motion, captures JPEG frames via ArduCAM, decompresses them with ChaN’s TJpgDec, and renders a 160×120 image on the LCD.  
- **Real‑Time GUI Alerts**  
  Flashes “INTRUDER” in red on the LCD using a high‑priority GUI task.  
- **Task Synchronization**  
  Coordinates PIR, camera, GUI, and button tasks with FreeRTOS semaphores and mutexes for safe resource sharing.  
- **Reset Button**  
  Resets the alert and clears the screen via a button‑monitoring task.  

---

## Hardware

- **MCU**: STM32F429I‑DISC1 (ARM Cortex‑M4)  
- **Sensors & Modules**:  
  - PIR motion sensor  
  - ArduCAM SPI camera module  
- **Display**: Integrated 240×320 LCD on STM32 board  
- **User Input**: On‑board reset button  

---

## Software & Libraries

- **RTOS**: FreeRTOS  
- **Toolchain**: GNU Arm Embedded / STM32CubeIDE or SW4STM32  
- **Languages**: C  
- **Drivers**: STM32 HAL (via CubeMX)  
- **JPEG Decompression**: ChaN’s TJpgDec library  
- **Serial Logging**: UART (e.g. Tera Term)  

---

## RTOS Task Architecture

| Task          | Priority  | Function                                                    |
|---------------|-----------|-------------------------------------------------------------|
| Button Task   | Highest   | Polls reset button (100 Hz); clears `intruderDetected`.     |
| PIR Task      | High      | Polls PIR sensor (10 Hz); sets/clears `intruderDetected`.    |
| Camera Task   | Medium    | Waits on flag; captures & decompresses JPEG; updates LCD.   |
| GUI Task      | Low       | Polls flag (10 Hz); flashes “INTRUDER” alert on LCD.        |

---

## Build & Deployment

1. **Clone Repo**  
   ```bash
   git clone <repo-url>
   cd realtime-security-system

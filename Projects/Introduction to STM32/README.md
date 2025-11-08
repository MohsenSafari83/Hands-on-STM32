# Introduction to STM32 & Blue Pill Basics (STM32F103C8T6)

This introductory section is dedicated to covering the fundamental concepts and hardware knowledge necessary to begin bare-metal and HAL programming on the **STM32F103C8T6 (Blue Pill)** development board.

Understanding these concepts is crucial before diving into peripheral configuration and coding.

## Learning Objectives

* Understand the naming convention and core specifications of the STM32F103C8T6 MCU.
* Identify the main components on the Blue Pill board and their functions (e.g., PC13 LED, BOOT jumpers).
* Grasp the chip's pin naming convention and key non-GPIO pins.
* Understand the role of STM32CubeMX and the HAL Libraries.

---

## The Chip

The core of our development board is the **STM32F103C8T6** microcontroller, based on the **ARM Cortex-M3** core. It is a 32-bit RISC processor known for its balance of performance and energy efficiency, making it highly popular among hobbyists and professionals.

Here is its specs:
![stm](images/stm-chip.webp)


### MCU Naming Convention Breakdown
| Part | Value | Meaning |
| :---: | :---: | :--- |
| **STM** | | Manufacturer (STMicroelectronics) |
| **32** | | 32-bit architecture |
| **F1** | **F1** Series | General Purpose (Mainstream) |
| **03** | **03** Connectivity | Includes USB and CAN peripherals |
| **C** | **C** | 48-pin package |
| **8** | **8** | Flash memory size (64 KB) |
| **T** | **T** | Package type (LQFP) |
| **6** | **6** | Operating temperature range (-40°C to +85°C) |


---

## Pin Naming Convention

Now let's take a look at the pinout of this chip, taken from the datasheet:


![Image of STM32F103C8T6 pinout diagram](images/pinout-stm.png)

### General Purpose Input/Output (GPIO) Overview

This section covers the fundamental role of GPIO pins in microcontrollers, their functionality, common uses, and the specific organization on the STM32F103C8T6 (Blue Pill) board.

## What is GPIO?

**GPIO (General Purpose Input/Output)** pins serve as the primary interface between the microcontroller (MCU) and the external world. They are versatile pins found on integrated circuits and circuit boards that can be used for various tasks, such as reading or sending digital signals. They make it possible to interact with sensors, actuators, and communication protocols.

The core functionality of a GPIO pin—whether it acts as an **input** (reading data) or an **output** (sending data)—is controlled entirely by **software**. A key feature of GPIO is the ability to dynamically change its function while the program is running by adjusting specific configuration settings (registers) within the MCU.

### Common Uses for GPIO Pins:

* **Reading** digital or analog values from external sensors.
* **Controlling** simple output devices (e.g., turning on/off LEDs, relays, motors).
* **Communicating** with other devices using single-wire protocols.
* **Handling Interrupts** (signals that require immediate, non-blocking attention from the CPU).

---

##  GPIO Pin Modes

The flexibility of STM32 GPIO pins comes from their ability to be configured into different operational modes via internal registers:

| Mode | Description | Common Use Cases |
| :---: | :--- | :--- |
| **Input** | Reads the logic level (high/low) from an external source. | Buttons, sensors, external signals |
| **Output** | Drives the pin high (VDD) or low (GND). | LEDs, relays, motor drivers, external devices |
| **Alternate Function** | Connects the pin to an on-chip peripheral (UART, SPI, I²C, etc.). | Communication interfaces, PWM, timers |
| **Analog** | Enables the pin for analog signals (ADC or DAC). | Analog sensors, voltage outputs |
| **Interrupt** | Triggers an event on signal transition (rising/falling edge). | Button presses, external event detection |

### Supplementary Pin Configurations

| Configuration | Description | Common Use Cases |
| :---: | :--- | :--- |
| **Pull-up/Pull-down** | Internal resistors to set default high/low when input is floating. | Pull-up for buttons, pull-down for inputs |
| **Open-drain** | Can only pull low; requires an external resistor to pull high. | I²C communication, multiple devices sharing a signal line |

---

##  Commonly Used GPIO Pins on Blue Pill (STM32F103C8T6)

Understanding which pins are multiplexed with common peripherals is critical for development:

| Pin(s) | Function | Notes |
| :---: | :--- | :--- |
| **PC13** | **Onboard LED** | Typically connected to the onboard user LED (Active-Low). |
| **PA0** | **ADC Channel** | Used for Analog-to-Digital Conversion (ADC) input. |
| **PA9 / PA10** | **USART1 (TX/RX)** | Standard pins for serial communication. |
| **PB6 / PB7** | **I2C1 (SCL/SDA)** | Used for I²C communication. |
| **PA5, PA6, PA7** | **SPI1 (SCK, MISO, MOSI)** | Used for high-speed Serial Peripheral Interface communication. |

***Voltage Note:** The GPIO pins on the STM32F103C8T6 are **3.3V tolerant**. **Connecting 5V directly to the GPIO pins can damage the microcontroller.** You must use a logic level shifter for 5V signals.*

---

## STM32F103C8T6 GPIO Organization

The STM32F103C8T6 microcontroller provides a total of **37 GPIO pins** available for general use, which are organized into three alphabetical ports:

1.  **Port A (GPIOA):** PA0 to PA15 (16 pins)
2.  **Port B (GPIOB):** PB0 to PB15 (16 pins)
3.  **Port C (GPIOC):** PC13 to PC15 (3 pins)
---
Unlike Arduino where pins are referred to by simply a number (pin 1, pin 2...), **GPIO pins** on bare microcontrollers like STM32 usually have a **port** and **number** associated with them. A **port** is a set of pins that are organized internally and can be controlled together.

In STM32, GPIO ports are named alphabetically starting from **A**, and each port can have up to 16 pins from **0** to **15**.

As a result, GPIO pins on STM32 are named like **PXY**, which stand for 'Port X Pin Y'. Due to size limits not all chips will have all the ports, and not every port will have all its 16 pins. In this case, most of the pins are from port A (PA0 to PA14). Port B only has 1 pin (PB1), and Port F has 2 (PF0 and PF1).

There are also some non-GPIO pins common to all STM32 chips that are worth mentioning:

| Pin name | Function |
| :---: | :--- |
| **VSS** | Ground |
| **VDD** | 3.3V Supply |
| **VDDA** | Analog supply for ADC and DAC. Usually equal to VDD. |
| **VBAT** | Battery input for RTC and low-power backups |
| **NRST** | Active low reset. Pulled up internally. |
| **BOOT0** | Boot mode. Low: normal startup, High: run bootloader |

---

## The Dev Board (Blue Pill)

The **Blue Pill** is a compact, cost-effective development board for the STM32F103C8T6.

![klajfk](images/blue-pill.jpg)



All in all a clean, simple, and versatile little board. Something to note:

* All pins have been broken out on the header.
* The **micro USB connector is for power only**, since this specific chip doesn't natively support USB capabilities for programming (you must use an ST-Link).
* All STM32 chips run at **3.3V**, but are **5V tolerant** on **digital** pins.
* Don't worry about the **BOOT0** selector, leave it on the default **GND side** (aka normal startup: running user code from Flash Memory).

---

## STM32CubeMX

**STM32CubeMX** is an interactive configuration tool and code generator provided by STMicroelectronics. It lets you set up the microcontroller's peripherals, clock tree, and pin assignments in a straightforward graphical interface. The tool then generates the initialization C code based on your settings, greatly simplifying the project setup phase.

[Click me](https://github.com/YOUR_USERNAME/hands-on-stm32/blob/main/Docs/Tool-Setup-Links.md) to download the latest version of STM32CubeIDE (which includes CubeMX) from the ST website.

## The STM32 HAL Libraries

**STM32 HAL (Hardware Abstraction Layer)** library is an open-source library written by ST and recommended for all new projects, and is what STM32CubeMX generates. It provides a complete set of APIs (e.g., `HAL_GPIO_WritePin()`) that stay consistent throughout the different STM32 lines. This simplifies the coding and improves portability across various STM32 microcontrollers.


# 01 - Introduction to STM32 & Blue Pill Basics (STM32F103C8T6)

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


![Image of STM32F103C8T6 pinout diagram](images/pinout-stm.jpg)


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


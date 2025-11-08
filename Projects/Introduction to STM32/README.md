#  Introduction to STM32 & Blue Pill Basics (STM32F103C8T6)

This introductory section is dedicated to covering the fundamental concepts and hardware knowledge necessary to begin bare-metal and HAL programming on the **STM32F103C8T6 (Blue Pill)** development board.

Understanding these concepts is crucial before diving into peripheral configuration and coding.
##  Learning Objectives

* Understand the naming convention and core specifications of the STM32F103C8T6 MCU.
* Identify the main components on the Blue Pill board and their functions (e.g., PC13 LED, BOOT jumpers).
* Grasp the high-level architecture of the ARM Cortex-M3 core.
* Comprehend the unified memory map and the process of addressing peripherals via their base addresses and offsets (Register Access).
## 1. ⚙️ Blue Pill Hardware Specifications

The Blue Pill board is centered around the **STM32F103C8T6** microcontroller:

### 1.1 MCU Naming Convention Breakdown
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

### 1.2 Key Specs and Features
| Feature | Detail |
| :--- | :--- |
| **Core** | ARM Cortex-M3 (32-bit RISC) |
| **Max Clock Speed** | 72 MHz |
| **Flash Memory (ROM)** | 64 KB (Program Storage) |
| **SRAM (RAM)** | 20 KB (Data Storage) |
| **GPIO Pins** | 37 General Purpose I/O pins |
| **On-board LED** | Connected to **PC13** (Active-Low: LED turns ON when the pin is pulled LOW). |
| **Oscillators** | 8 MHz High-Speed External (HSE) and 32.768 KHz Low-Speed External (LSE) |

### 1.3 BOOT Jumpers
The two jumpers (**BOOT0** and **BOOT1**) control the memory from which the CPU fetches instructions after a reset:

| BOOT1 | BOOT0 | Boot Mode | Description |
| :---: | :---: | :--- | :--- |
| X | **0** | **Main Flash Memory** | **Default mode** for running the user application code. |
| 0 | **1** | **System Memory** | Used for programming the MCU via Serial/UART using the built-in ST bootloader. |
| 1 | **1** | Embedded SRAM | Used primarily for debugging purposes. |

---

## 2. 🧱 STM32 Architecture and Memory Mapping

### 2.1 ARM Cortex-M3 Core Overview
The STM32F103C8T6 utilizes the **Cortex-M3** core, known for its balance of performance and efficiency. Key architectural aspects include:

* **Registers:** The core uses 16 registers (R0-R15). R13 (Stack Pointer), R14 (Link Register), and R15 (Program Counter) are essential for function calls, stack management, and instruction fetching.
* **Nested Vectored Interrupt Controller (NVIC):** A component that manages all interrupt requests (IRQs) from peripherals, prioritizing them and routing them to the CPU.
* **Bus Matrix:** A system of high-speed buses (AHB/APB) that connects the CPU core (Master) to various memories and peripherals (Slaves), allowing simultaneous data transfers.

### 2.2 Unified Memory Map
The Cortex-M3 core uses a **unified 4 GB address space** where everything—Flash, SRAM, and Peripherals—is treated as memory and accessed via a unique 32-bit address.

The 4 GB address space is divided into 8 main blocks (each 512 MB):

| Start Address | End Address | Block (512 MB) | Contents |
| :---: | :---: | :--- | :--- |
| **0x0000 0000** | 0x1FFF FFFF | Code | **Flash Memory (ROM)** and System Memory. This is where your compiled program code resides. |
| **0x2000 0000** | 0x3FFF FFFF | SRAM | **SRAM (RAM)**. Used for dynamic data, global variables, and the stack/heap. |
| **0x4000 0000** | 0x5FFF FFFF | Peripherals | **Peripherals.** All peripheral control registers (GPIO, UART, Timer, etc.) are mapped here. |
| 0x6000 0000 | 0xFFFF FFFF | External Devices | Reserved for external memory or manufacturer-specific use. |

### 2.3 Register Access (The Key to Programming)
To control a peripheral (e.g., turn on a pin via GPIO), you must write a specific value to a specific address in the Peripherals block.

* **Base Address:** Each peripheral (like GPIOA, GPIOB, or RCC) has a unique starting address in the memory map (e.g., `0x4001 0800` for GPIOA).
* **Register Offset:** Control functions are achieved by accessing Registers, which are located at an *offset* from the peripheral's base address.
    * **Example (Conceptual):** To control the GPIO Port C Output Data Register (`ODR`), you would access the address: `(GPIOC_BASE_ADDRESS + ODR_OFFSET)`.

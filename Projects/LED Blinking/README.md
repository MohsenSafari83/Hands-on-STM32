# 💡 STM32 LED Blink Project

This is the first practical project after learning the basic STM32 concepts, the details of which you can find in the [Introduction to STM32](https://github.com/MohsenSafari83/Hands-on-STM32/tree/main/Projects/Introduction%20to%20STM32) repository.

---

##  Table of Contents
* [STM32 LED Blink Overview](#stm32-led-blink-overview)
* [STM32 LED Blink Example](#stm32-led-blink-example)
    * [Objectives of This Project](#objectives-of-this-project)
    * [Step \#1: Selecting MCU](#step-1-selecting-mcu)
    * [Step \#2: Pinout Configuration](#step-2-pinout-configuration)
    * [Step \#3: Clock Configuration (72MHz)](#step-3-clock-configuration-72mhz)
    * [Step \#4: Project Generation](#step-4-project-generation)
    * [Step \#5: Application Code](#step-5-application-code)
* [Hardware Connection](#hardware-connection)
* [Required Parts For STM32](#required-parts-for-stm32)

---

## STM32 LED Blink Overview

To create an STM32 LED Blink project, we need to configure a **GPIO** pin as an **Output** pin and toggle its state at fixed time intervals.

### Pin State Change Functions
We can use the following functions for the GPIO pin state change:

* `HAL_GPIO_WritePin()`: Sets an output pin to **HIGH** (SET) or **LOW** (RESET).
* `HAL_GPIO_TogglePin()`: **Toggles** the current state of a GPIO pin.

### Delay Options
For the delay between each pin state change, we can use the following options:

* `HAL_Delay()`: Built-in Function (Easiest for beginners).
* STM32 Hardware Timer Delay
* STM32 SysTick Timer Delay
* STM32 DWT Delay

The **`HAL_Delay()`** function is the easiest to use for such a project, especially if it’s your first STM32 project. However, other time delay techniques are illustrated in more detail in the links above.

---

## STM32 LED Blink Example

### Objectives of This Project
The objectives of this STM32 LED Blink example project are:
* Configure **GPIO Output Pin** within the **STM32CubeMX** Tool.
* Use `HAL_GPIO_WritePin()` function to change an output pin state.
* Use `HAL_GPIO_TogglePin()` function to toggle the state of a GPIO pin.
* Use The `HAL_Delay()` and understand how it works.

### Step \#1: Selecting MCU
Open **STM32CubeMX**, create a new project, and select the **STM32F103C8T6** target microcontroller. Note that the STM32 BluePill board has two common target microcontrollers (STM32F103C8T6 & STM32F103C6T6). So you need to select the exact target microcontroller on your hardware board.
This example project should work flawlessly on any STM32 target microcontroller; you just need to select the target MCU that matches your hardware board.

### Step \#2: Pinout Configuration
1.  Go to the **RCC** clock configuration page and enable the **HSE external crystal oscillator** input.
2.  Click on the **PB11** GPIO pin in the “Pinout View” and select it to be in **GPIO\_Output** mode. Note: you can use any other pin you want instead.

### Step \#3: Clock Configuration (72MHz)
1.  Go to the clock configurations page, and select the **HSE** as a clock source and **PLL** output.
2.  Type in **72MHz** for the desired output system frequency. Hit the “Enter” key, and let the application solve for the required PLL dividers/multipliers.
    > The reason behind this: using the external onboard oscillator on the BluePill board provides a more accurate and stable clock, and using a **72MHz** as a system clock pushes the microcontroller to its limits, so we get the maximum performance out of it (as long as we don’t care about the application’s power consumption). 

---

### Step \#4: Project Generation
1.  Finally, go to the **Project Manager**.
2.  Give your project a name.
3.  Select the toolchain/IDE to be **STM32CubeIDE**.
4.  Click on the **Generate Code** button.

The STM32CubeMX tool will generate the initialization code & the project main files. It will prompt you to open the project in STM32CubeIDE. Select, open project, and then head over to the **`main.c`** file to start writing the application code.

### Step \#5: Application Code
Copy the following code into your **`main.c`** file, replacing the auto-generated code from the beginning of the file up to the `main()` function. You should leave everything else under the `main()` function in the `main.c` file as is.

#### STM32 LED Blink Example Code (HAL GPIO Write Pin)
This code example uses the STM32 `HAL_GPIO_WritePin()` function.

```c
#include "main.h"
 
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
 
int main(void)
{
  /* Initialize the HAL Library, system clock, and peripherals */
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
 
  /* Application Infinite Loop */
  while (1)
  {
    // LED ON (Set pin PB11 to HIGH)
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_11, GPIO_PIN_SET);
    HAL_Delay(100);
    
    // LED OFF (Set pin PB11 to LOW)
    HAL_GPIO_WritePin(GPIOB, GPIO_PIN_11, GPIO_PIN_RESET);
    HAL_Delay(100);
  }
}
```
### Alternative: Using HAL_GPIO_TogglePin()
If you prefer to use the `HAL_GPIO_TogglePin()` function, you can rewrite the while(1) loop as follows:

```c
/* Application Infinite Loop */
  while (1)
  {
    HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_11);
    HAL_Delay(200); // 200ms delay for each state change
  }
```
## 🔌 Hardware Connection

To program the code onto the STM32 board and observe the LED's operation, follow these connection instructions:

### 1. Programming Connection (ST-Link to STM32)

Connect the **ST-Link V2 programmer** to the USB port and use its SWD (Serial Wire Debug) pins to connect to the corresponding pins on the STM32 board (e.g., BluePill). This allows the IDE to upload the compiled code.

* **GND** $\rightarrow$ **GND**
* **SWCLK** $\rightarrow$ **SWCLK (Pin A14)**
* **SWDIO** $\rightarrow$ **SWDIO (Pin A13)**
* **VCC/3.3V** $\rightarrow$ **3.3V** (This powers the board; optional if powered via USB, but recommended for stability.)

![Programming Connection](images/)

### 2. LED Circuit Connection

Connect the **LED** to the configured output pin (**PA8**) using a **current-limiting resistor** (e.g., 220 Ohms). This resistor is crucial to prevent the LED from drawing too much current and being damaged.

* **PA8** $\rightarrow$ **Resistor** $\rightarrow$ **LED Anode** (Longer Leg) $\rightarrow$ **LED Cathode** (Shorter Leg) $\rightarrow$ **GND**



---

##  Required Parts For STM32

The following components are required to complete this STM32 LED Blink project:

| Part | Quantity | Description |
| :--- | :--- | :--- |
| **STM32 BluePill Board** | 1 | Development board based on STM32F103C8T6 |
| **ST-Link V2 Programmer** | 1 | Used to upload code to the microcontroller |
| **LED** | 1 | Light Emitting Diode (Any color) |
| **Resistor** | 1 | 220 Ohm Resistor (Crucial for LED protection) |
| **Breadboard & Wires** | Varies | For connecting components and prototyping |



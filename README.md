# Falcon F4 Flight Controller

Custom STM32F405-based flight controller designed for UAV, FPV and aerial robotics applications.

## Overview

The Falcon F4 Flight Controller is a custom-designed flight-control board based on the STM32F405RGT6 ARM Cortex-M4 microcontroller.

The board integrates the core hardware required for a compact UAV flight-control system, including a 6-axis IMU, barometric pressure sensor, analog video OSD, external SPI Flash memory, regulated power supplies, battery-voltage monitoring, current sensing, ESC interfaces, camera and VTX interfaces, USB connectivity and SWD debugging.

The complete electrical schematic is provided in this repository along with images of the designed PCB.

## Key Features

* STM32F405RGT6 ARM Cortex-M4 microcontroller
* ICM-42688-P 6-axis IMU
* BMP388 barometric pressure sensor
* AT7456E analog OSD controller
* W25Q128JV 128-Mbit SPI NOR Flash
* TPS54202DDCR 5 V buck regulator
* XC6206P332MR 3.3 V LDO regulator
* USB Type-C interface
* SWD programming and debugging interface
* Dedicated boot-mode selection
* Multiple ESC signal outputs
* ESC telemetry interface
* Analog camera input
* Analog VTX output
* Battery-voltage monitoring
* Current-sense input
* SPI interfaces
* I²C interface
* Status LEDs
* Power indication
* Dedicated MCU and OSD clock sources

## Main Processing Unit

### STM32F405RGT6

The STM32F405RGT6 is the main processing unit of the Falcon F4.

It provides the processing capability required for real-time flight-control applications and interfaces with the onboard sensors, memory, OSD circuitry, external peripherals and flight-control interfaces.

The MCU provides connections for:

* IMU
* Barometer
* SPI Flash
* OSD
* ESC interfaces
* Battery-voltage monitoring
* Current sensing
* I²C peripherals
* USB
* SWD
* Status LEDs
* Auxiliary GPIO

## Inertial Measurement Unit

### ICM-42688-P

The ICM-42688-P is used as the primary inertial measurement unit.

It provides:

* 3-axis accelerometer
* 3-axis gyroscope

The IMU is connected to the STM32 through an SPI interface.

The main IMU signals are:

```text
SPI1_SCK
SPI1_MISO
SPI1_MOSI
IMU_CS
```

The IMU has local decoupling capacitors to provide a stable supply and reduce supply noise.

The sensor provides motion information required for:

* Attitude estimation
* Angular-rate measurement
* Flight stabilization
* Orientation estimation
* Flight-control algorithms

## Barometric Pressure Sensor

### BMP388

The BMP388 is used for barometric pressure and altitude measurement.

The sensor is connected to the STM32 through the I²C interface.

The primary signals are:

```text
I2C1_SCL
I2C1_SDA
```

The barometer can be used for:

* Altitude estimation
* Altitude hold
* Vertical-position estimation
* Flight stabilization
* Autonomous UAV applications

## On-Screen Display

### AT7456E

The AT7456E is used as the analog video OSD controller.

The OSD section allows flight information to be overlaid onto the analog video stream.

The video path is:

```text
Camera
   |
   v
AV_IN
   |
   v
AT7456E
   |
   v
AV_OUT
   |
   v
VTX
```

The OSD section includes a dedicated 27 MHz crystal and associated circuitry.

Possible OSD information includes:

* Battery voltage
* Flight mode
* Altitude
* Flight status
* Warnings
* Navigation information
* Telemetry information
* Artificial horizon

## External Flash Memory

### W25Q128JV

The W25Q128JV is a 128-Mbit SPI NOR Flash memory device.

The external Flash provides non-volatile memory for storing application data.

Potential uses include:

* Configuration data
* Parameters
* Logging
* Flight data
* Application-specific information

The Flash interface uses:

```text
FLASH_CS
SPI3_SCK
SPI3_MISO
SPI3_MOSI
```

## Power Management

The Falcon F4 contains dedicated power-management circuitry for generating the required voltage rails.

The main power architecture consists of:

```text
+BATT
   |
   +------------------+
   |                  |
   v                  v
5 V Buck          Battery
Regulator         Monitoring
   |
   v
  5 V
   |
   v
3.3 V LDO
   |
   v
 3.3 V
```

## 5 V Regulator

### TPS54202DDCR

The TPS54202DDCR is used as the primary 5 V switching regulator.

The regulator converts the battery input into a regulated 5 V supply.

The regulator section includes:

* TPS54202DDCR
* Inductor
* Input capacitors
* Output capacitors
* Feedback network
* Enable circuitry
* Power flag

The 5 V rail supplies the OSD/video section and other peripherals requiring 5 V.

## 3.3 V Regulator

### XC6206P332MR

The XC6206P332MR provides the 3.3 V supply rail.

The 3.3 V rail powers the main low-voltage digital and sensor circuitry, including:

* STM32F405RGT6
* ICM-42688-P
* BMP388
* W25Q128JV
* Other 3.3 V peripherals

Local decoupling capacitors are provided for the regulator and individual ICs.

## Battery Voltage Monitoring

The Falcon F4 includes an onboard battery-voltage sensing circuit.

A resistor-divider network scales the battery voltage to a suitable level for the STM32 ADC.

The schematic uses:

```text
R18 = 20 kΩ
R19 = 1 kΩ
```

The scaled signal is connected to:

```text
ADC_IN8
```

This allows the flight-control firmware to monitor the connected battery voltage.

Battery-voltage monitoring can be used for:

* Battery status
* Low-voltage warnings
* Battery protection
* Flight telemetry
* Battery monitoring

## Current Monitoring

A dedicated current-sense input is provided.

The signal is labelled:

```text
CURRENT
```

and is connected to the STM32 ADC system.

The current measurement can be used for:

* Current monitoring
* Power-consumption estimation
* Battery monitoring
* Energy-consumption calculation
* Telemetry
* Low-battery management

The actual current-sensor implementation depends on the external ESC or power-monitoring hardware connected to the flight controller.

## ESC Interface

The Falcon F4 provides a dedicated multi-pin ESC interface.

The schematic includes:

```text
ESC_S1
ESC_S2
ESC_S3
ESC_S4
ESC_S5
ESC_S6
ESC_TLM
CURRENT
+BATT
GND
```

The interface is intended to connect the flight controller to external electronic speed controllers.

The multiple ESC signal connections allow the board to be adapted for different multirotor configurations.

The ESC interface can be used for:

* Motor-control signals
* ESC telemetry
* Current monitoring
* Battery monitoring

The exact communication protocol is determined by the flight-control firmware and connected ESC hardware.

## Camera Interface

A dedicated analog camera interface is provided.

The camera connector includes:

```text
GND
+5V
AV_IN
```

The camera video signal is routed to the AT7456E OSD section.

The OSD then overlays flight information onto the video stream before sending it to the VTX.

## VTX Interface

A dedicated VTX connector is provided.

The interface includes:

```text
GND
+BATT
AV_OUT
VTX_TBS
```

The processed analog video signal is available through:

```text
AV_OUT
```

The VTX control signal can be used with compatible video transmitters and firmware.

## USB Type-C Interface

The Falcon F4 includes a USB Type-C connector.

The USB section includes the required USB-C CC resistors and connections to the STM32.

The interface can be used for:

* Firmware development
* Configuration
* Data communication
* Firmware updates
* Development and testing

## Debugging and Programming

### SWD

A dedicated Serial Wire Debug interface is provided for programming and debugging the STM32F405.

The primary SWD signals are:

```text
SWDIO
SWCLK
```

The SWD interface can be used with an ST-LINK or compatible debugger.

It supports:

* Firmware programming
* Breakpoint debugging
* Register inspection
* Memory inspection
* Peripheral debugging
* Hardware bring-up

## Boot Mode

The board includes a dedicated boot-selection circuit.

The schematic provides:

```text
BOOT
BOOT0
```

along with a boot switch and associated pull-down circuitry.

This allows the MCU boot configuration to be selected during development and firmware recovery.

## Status LEDs

The Falcon F4 includes onboard status and power indication LEDs.

The schematic contains:

```text
LED1
LED2
POWER
```

These LEDs can be assigned by firmware to indicate:

* Power status
* Boot status
* Sensor initialization
* Flight-controller state
* Communication status
* Error conditions

## Clock Sources

The board contains dedicated clock sources for the MCU and OSD subsystem.

### MCU Clock

An 8 MHz crystal is provided for the STM32F405 clock system.

### OSD Clock

A separate 27 MHz crystal is provided for the AT7456E OSD controller.

## Communication Interfaces

### SPI

Multiple SPI interfaces are used for high-speed peripheral communication.

The SPI interfaces are used for devices including:

* ICM-42688-P
* W25Q128JV
* Other external peripherals

### I²C

The board provides an I²C interface for peripheral communication.

The primary signals are:

```text
I2C1_SCL
I2C1_SDA
```

The onboard BMP388 is connected through this interface.

## Major Components

| Reference | Component           | Function                      |
| --------- | ------------------- | ----------------------------- |
| U1        | STM32F405RGT6       | Main flight-controller MCU    |
| U2        | BMP388              | Barometric pressure sensor    |
| U3        | ICM-42688-P         | 6-axis IMU                    |
| U4        | W25Q128JV           | 128-Mbit SPI Flash            |
| U5        | TPS54202DDCR        | 5 V buck regulator            |
| U6        | XC6206P332MR        | 3.3 V LDO regulator           |
| IC1       | AT7456E             | Analog OSD controller         |
| Y1        | 27 MHz crystal      | OSD clock source              |
| Y2        | 8 MHz crystal       | MCU clock source              |
| J1        | USB Type-C          | USB interface                 |
| J2        | ESC connector       | ESC interface                 |
| J3        | VTX connector       | VTX interface                 |
| J4        | Camera connector    | Camera interface              |
| J5        | Auxiliary connector | External peripheral interface |
| SW1       | Boot switch         | MCU boot selection            |


## Intended Applications

The Falcon F4 is intended as a hardware platform for:

* FPV drones
* Multirotor UAVs
* Research UAVs
* Autonomous aerial vehicles
* Flight-control development
* UAV research
* Embedded systems development
* UAV education
* Custom flight-control firmware
* Robotics applications
* Experimental aircraft

## Hardware Images

### PCB Front View

![Falcon F4 Flight Controller - Front View](Images/PCB%20FRONT%20VIEW.png)

### PCB Back View

![Falcon F4 Flight Controller - Back View](Images/PCB%20BACK%20VIEW.png)

## Schematic

The complete electrical schematic is provided in this repository.

The schematic contains the major functional sections of the flight controller:

* USB interface
* Battery input
* 5 V power regulation
* 3.3 V regulation
* STM32F405RGT6 MCU
* ICM-42688-P IMU
* BMP388 barometer
* AT7456E OSD
* W25Q128JV Flash
* ESC interface
* Camera interface
* VTX interface
* Battery-voltage sensing
* Current sensing
* SWD interface
* Boot circuit
* Status LEDs
* SPI interfaces
* I²C interface

## Repository Structure

```text
Falcon-F4-Flight-controller/
│
├── README.md
│
├── Schematic/
│   └── Falcon F4 Flightcontroller.kicad_sch
│
└── Images/
    ├── PCB FRONT VIEW.png
    └── PCB BACK VIEW.png
```

## Development Status

Current project status:

**Hardware schematic completed.**

The PCB design has been developed as a custom flight-controller platform based on the STM32F405RGT6.

The repository currently contains the electrical schematic and PCB documentation images.

Further development may include PCB manufacturing files, bill of materials, assembly documentation and firmware.

## Development and Testing

Recommended hardware bring-up sequence:

```text
Schematic Verification
        |
        v
PCB Fabrication
        |
        v
PCB Assembly
        |
        v
Visual Inspection
        |
        v
Power-Rail Verification
        |
        v
STM32 Programming
        |
        v
USB Verification
        |
        v
IMU Verification
        |
        v
Barometer Verification
        |
        v
Flash Verification
        |
        v
OSD and Video Verification
        |
        v
ESC Interface Verification
        |
        v
Flight-Control Firmware
        |
        v
UAV Integration
```

Initial testing should verify:

* Battery input
* 5 V rail
* 3.3 V rail
* MCU operation
* MCU reset
* MCU clock
* USB communication
* SWD communication
* IMU communication
* Barometer communication
* Flash communication
* Battery-voltage ADC measurement
* Current measurement
* ESC outputs
* OSD video input and output

## Safety and Testing Notice

This is a custom flight-controller hardware project.

The hardware should be thoroughly tested before installation on a flight-capable UAV.

Initial testing should be performed without propellers and with appropriate current limiting and laboratory power equipment.

Particular attention should be given to:

* Battery polarity
* Power-rail voltage
* Regulator temperature
* MCU temperature
* Sensor communication
* ADC voltage scaling
* ESC outputs
* Video signal integrity
* Connector polarity
* Short circuits
* PCB assembly quality

## Design Philosophy

The Falcon F4 was designed around four primary objectives.

### Performance

The STM32F405RGT6 provides a capable real-time processing platform suitable for UAV flight-control applications.

### Integration

The board integrates inertial sensing, barometric sensing, OSD, external Flash memory and power regulation into a single custom flight-controller platform.

### Expandability

SPI, I²C, ADC, GPIO and dedicated external connectors provide flexibility for additional sensors, ESCs, cameras, VTX systems and peripherals.

### Development

USB, SWD and BOOT interfaces simplify firmware development, debugging and hardware testing.

## Project Information

| Parameter       | Details                     |
| --------------- | --------------------------- |
| Project Name    | Falcon F4 Flight Controller |
| MCU             | STM32F405RGT6               |
| IMU             | ICM-42688-P                 |
| Barometer       | BMP388                      |
| OSD             | AT7456E                     |
| External Flash  | W25Q128JV                   |
| 5 V Regulator   | TPS54202DDCR                |
| 3.3 V Regulator | XC6206P332MR                |
| PCB Design Tool | KiCad                       |
| Application     | UAV / FPV / Flight Control  |

## License

The licensing terms for the hardware design files and associated documentation will be specified as the project develops.

## Project

Falcon F4 Flight Controller

Custom STM32F405-based flight-control hardware platform for UAV, FPV and aerial robotics applications.

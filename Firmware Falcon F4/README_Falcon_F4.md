# Falcon F4 Flight Controller Firmware

The [Betaflight](https://github.com/betaflight/betaflight) firmware was used and tested for the Falcon F4 flight controller. The compiled `betaflight_4.5.2_Falcon_F4.hex` file can be used to flash the firmware. The configuration files are also available in the repository in case you want to compile the firmware yourself.

## How to Flash

The easiest way of flashing the firmware is using STM32CubeProgrammer. Other methods also exist, e.g. using [ST-LINK Utility](https://github.com/stlink-org/stlink) or [OpenOCD](https://github.com/openocd-org/openocd).

### Step 1

Install [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html).

### Step 2

While holding the **BOOT/DFU button** on the Falcon F4 flight controller, plug in the USB-C cable and connect the flight controller to the computer. Release the button after connecting.

This puts the STM32 microcontroller into **DFU mode** and allows the firmware to be flashed over USB.

### Step 3

Open STM32CubeProgrammer. From the connection dropdown, select **USB**. A DFU device should appear in the **Port** section.

If the device does not appear, disconnect the USB cable and repeat **Step 2**.

<div style="text-align:center;">
<img src="../../images/dfu.png" alt="STM32CubeProgrammer DFU" style="width: 300px;">
</div>

Click **Connect**.

### Step 4

Go to the **Erasing & Programming** tab and click **Full chip erase**.

<div style="text-align:center;">
<img src="../../images/programming.png" alt="STM32CubeProgrammer Programming tab" style="width: 600px;">
</div>

After the chip erase is complete, select the following firmware file included in the repository:

`betaflight_4.5.2_Falcon_F4.hex`

Click **Start Programming** and wait until the firmware has been completely flashed.

### Done

After programming is complete, disconnect and reconnect the USB cable.

The Falcon F4 flight controller should now boot with the flashed Betaflight firmware. You should be able to connect to it using the [Betaflight App](https://github.com/betaflight/betaflight-configurator).

<div style="text-align:center;">
<img src="../../images/betaflight_configurator.png" alt="Betaflight Configurator" style="width: 600px;">
</div>

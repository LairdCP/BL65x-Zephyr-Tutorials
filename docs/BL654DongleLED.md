# How to map LED0 to the BL654 Dongle's (451-00004) LED

Currently, the BL654 dongle (Part Number: 451-00004) does not have its own Zephyr board files. However, by using [devicetree overlays](https://docs.zephyrproject.org/latest/guides/dts/howtos.html#set-devicetree-overlays), we can use Nordic Semiconductor's board file and make it work with the BL654 Dongle.

In this tutorial, we will show you how to create an overlay file to remap the correct GPIO to the BL654 dongle's LED. We will then build the Blinky sample app and flash it into the BL654 dongle.

If you are not familiar with devicetrees, please refer to [Zephyr Devicetree](https://docs.zephyrproject.org/latest/guides/dts/index.html).



1. Prerequisites

   - You followed our [Zephyr Getting Started Guide](ubuntu.md) on how to setup the Zephyr development environment (i.e. Steps 1 - 5)

   - Install nRFutil

     ```
	 pip3 install nrfutil
     ```

     nRFutil is a command-line utility that supports Device Firmware Updates (DFU). We will use it to generate a firmware update package and flash the package into the dongle.
   
     

2. Create a devicetree overlay

   - Create a directory named "boards" in the samples/basic/blinky  directory

   - With a text editor, create a file named "nrf52840dongle_nrf52840.overlay" in the samples/basic/blinky/boards

   - Copy and paste the code below onto the overlay. This remaps LED0 to GPIO 13 and makes it active high.

     ```
     &led0_green{ 
         gpios = <&gpio0 13 GPIO_ACTIVE_HIGH>; 
     };
     ```
   
   - Save the file

3. Build Blinky

   ```
   cd ~/zephyrproject/zephyr
   west build -p auto -b nrf52840dongle_nrf52840 samples/basic/blinky 
   ```
   
 
   
4. Create DFU package

   ```
   nrfutil pkg generate --hw-version 52 --application build/zephyr/zephyr.hex --application-version 1 --sd-req 0x00 blinky.zip 
   ```
   
   


5. Flash the update package into the dongle

   - Plug the dongle into the PC and put it into Bootloader Mode

     To put into Bootloader Mode, press the reset button on the dongle. While in Bootloader Mode, you should  see the LED fade in and out slowly. 

     

     ![ResetButton](../images/dongle/ResetButton.PNG)

   - Determine which USB port to use for flashing

     ```
     dmesg
     ```

     Whenever the dongle goes into Bootloader Mode, it should enumerate as a USB device. When you do a dmesg, you should see something similar to the screenshot below. In this case, we have port ttyACM0; so we will use /dev/ttyACM0 port for flashing. Your PC might have a different port.

     

     ![ttyACM0](../images/dongle/ttyACM0.png)

     

   - Flash the build

     ```
     nrfutil dfu serial -pkg blinky.zip -p /dev/ttyACM0 
     ```
     
     
     
     If  flashing is successful, you should now see the LED blink at a rate of 1 per second (i.e. it should not be fading in/out).
     
     
     
     If you get a Permission Denied error as shown below, you need to add your username to the dialout group.
     
     
     
     ![PermissionError13](../images/dongle/PermissionError13.png)
     
     
     
     To add your username to the dialout group, do the following:
     
     ```
     sudo adduser your_username dialout 
     ```
     
     
     
     

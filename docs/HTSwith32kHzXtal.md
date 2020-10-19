# Enabling the 32kHz Crystal on the BL65x Dev Kit

For designs that require the least amount of current consumption, it's recommended that you use an external +/- 20ppm 32.768kHz crystal. Laird's BL65x dev kit comes with the 32.768kHz crystal. However, the default setting is not connected (i.e. configured for the internal RC). This tutorial will show you how to connect the 32.768 kHz crystal, and then build the Health Thermometer sample app so that it works with the external 32.768kHz crystal.



1. Prerequisites

   - You have followed our [Zephyr Getting Started Guide](ubuntu.md).

   - Install [nRF Toolbox](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Toolbox) into an Android or iOS device.

     

2. Connect the 32.768kHz Crystal

   - Unsolder R127 and R128 from the dev kit

   - Short Solder Bridge SB8 and SB9

     

     ![](../images/xtal/SolderBridge.PNG)

     

3. Create a .conf file to enable external 20ppm crystal

   - With a text editor, create a .conf file in the samples/bluetooth/peripheral_ht /boards directory. If the "boards" directory does not, exist create it.

     For the BL652, create bl652_dvk.conf

     For the BL653, create bl653_dvk.conf

     For the BL654, create bl654_dvk.conf

     

   - Copy and paste code below onto the .conf file you created

     ```
     # 32kHz clock source 
     CONFIG_CLOCK_CONTROL_NRF_K32SRC_XTAL=y 
     CONFIG_CLOCK_CONTROL_NRF_K32SRC_20PPM=y 
     ```

     

   - Save the file

     

4. Build the Health Thermometer App

   - Build for BL652

     ```
     cd ~/zephyrproject/zephyr
     west build -p auto -b bl652_dvk samples/bluetooth/peripheral_ht 
     ```

   - Build for BL653

     ```
     cd ~/zephyrproject/zephyr
     west build -p auto -b bl653_dvk samples/bluetooth/peripheral_ht
     ```

   - Build for BL654

     ```
     cd ~/zephyrproject/zephyr
     west build -p auto -b bl654_dvk samples/bluetooth/peripheral_ht
     ```




5. Flash the build

   - Connect PC to USB2 port of the dev kit

   - Flash the build

     ```
     nrfjprog -e
     west flash
     ```
     
     After flashing, the module should automatically go into advertising mode.
     
     


6. Connect to the module using nRF Toolbox

   - Launch nRFToolbox and then click HTM icon

     ![](../images/xtal/ClickHTM.png)

     

   - Click connect 

     ![](../images/xtal/Connect.png)

     

   - Select the Zephyr Health Thermometer device

     ![](../images/xtal/ClickHTS.png)

   - Temp shown

     ![](../images/xtal/Temp.png)

     

   


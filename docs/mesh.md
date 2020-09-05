# Getting Started with Mesh

This tutorial will demonstrate how to bring up a Generic OnOff Model Server on a BL65x dev kit. We will provision three nodes into the network. All three nodes will be configured as Generic OnOff Model Servers. To allow us to simultaneously toggle LED1 on/off on all nodes, the nodes will also be configured to subscribe to a group.

The goal of this tutorial is to give you a simple example that can get you started on mesh right away. For a deeper dive into Bluetooth Mesh,  refer to the [Bluetooth Mesh Developer Study Guide](https://www.bluetooth.com/blog/bluetooth-mesh-developer-study-guide-v2-0/). 



1. Prerequisites

   - You have followed our [Zephyr Getting Started Guide](ubuntu.md).

   - Install [nRF Mesh](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Mesh) into an Android or iOS device. This will be used to provision and configure the nodes, and to turn on/off LED1 on the dev kit.

   - You have three BL65x dev kits. Two dev kits will also work. For this demo, we will use a BL562 (node1), BL653 (node2) and BL654 (node3). 

     

2. Build the Mesh Sample app

   1. Create a folder named "laird" in the ~/zephyrproject folder.

   2. Copy  the mesh application folder from ~/zephyrproject/zephyr/samples/bluetooth/mesh  into the ~/zephyrproject/laird folder you just created. Since we'll be changing main.c, it's a good idea to create a separate application folder. This way, we always have a copy of the original.

   3. Replace the main.c from ~/zephyrproject/laird/mesh/src/main.c with this [main.c](../src/mesh/main.c)

      This replacement main.c is basically the original main.c with the following modifications:

      - Added code to control LED1 of the dev kit. Code is based on the Blinky example

      - Added UUID. NOTE: We will program each node with a different UUID

      - Added code to handle an unacknowledged on/off message
      - Enabled relay function

      It's recommend that you diff the original main.c (i.e. the one in ~/zephyrproject/zephyr/samples/bluetooth/mesh/src) against the replacement main.c. This will allow you to easily see the changes that were done.

   4. Open replacement main.c with a text editor and confirm that the UUID is set for node 1

      ```c
      static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x01 }; //use for node 1
      //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x02 }; //use for node 2
      //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x03 }; //use for node 3
      ```

      

   5. Build and flash node 1

      For this demo we will use the BL652 dev kit for node 1

      - To build for BL652

        ```
        cd ~/zephyrproject/laird
        west build -p auto -b bl652_dvk mesh 
        ```

      - To flash connect PC to USB2 port of dev kit

        Before flashing make sure you fully erase the module first
        
        ```
        nrfjprog -e
        west flash
        ```
        

   6. Build and flash for node 2

      For this demo will use the BL653 dev kit for node 2

      - Modify main.c to use the node 2 UUID

        ```c
        //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x01 }; //use for node 1
        static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x02 }; //use for node 2
        //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x03 }; //use for node 3
        ```

        

      - To build for BL653

        ```
        cd ~/zephyrproject/laird
        west build -p auto -b bl653_dvk mesh 
        ```

      - To flash connect PC to USB2 port of dev kit

        Before flashing make sure you fully erase the module first 
        
        ```
        nrfjprog -e
        west flash
        ```

   7. Build and flash for node 3

      For this demo will use the BL654 dev kit for node 3

      - Modify main.c to use the node 3 UUID

        ```c
        //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x01 }; //use for node 1
        //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x02 }; //use for node 2
        static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x03 }; //use for node 3
        ```

        

      - To build for BL654 

        ```
        cd ~/zephyrproject/laird
        west build -p auto -b bl654_dvk mesh 
        ```

      - To flash connect PC to USB2 port of dev kit

        Before flashing make sure you fully erase the module first 
        
        ```
        nrfjprog -e
        west flash
        ```

   

4. Provision nodes, enable Generic On Off Model  and setup a Subscription

   - Now that all nodes are flashed, power up all 3 nodes.
   
   - Optional: If you are powering the dev kit via the USB1 port, you can monitor debug messages with a terminal emulator (e.g. Tera Term) set to 115200, N, 8, 1. 
   
   - Launch the nRF Mesh and then click "ADD NODE". You should see the 3 nodes as shown below. To provision, click one of the nodes.
   
     ![3Nodes](../images/mesh/3Nodes.png)
   
     
   
   - Next click "IDENTIFY"
   
     ![AddNode1](../images/mesh/AddNode1.png)
   
     
   
   - Next click "PROVISION"
   
     ![Provision](../images/mesh/Provision.png)
   
     
   
   - Select "No OOB" and then click "OK"
   
     ![OOB](../images/mesh/OOB.png)
   
     
   
   - Provision complete, click "OK"
   
     ![ProvisionComplete](../images/mesh/ProvisionComplete.png)
   
     
   
   - Next click the provisioned node
   
     ![ClickProvsionedNode](../images/mesh/ClickProvsionedNode.png)
   
     
   
   - Click node name to rename. We will change it from "Zephyr" to "BL653".
   
     ![NodeName](../images/mesh/NodeName.png)
   
     
   
     ![OldName](../images/mesh/OldName.png)
   
     
	
     ![NewName](../images/mesh/NewName.png)
   
     
	
   - Next expand "Elements" pull down menu
   
     ![ExpandElement](../images/mesh/ExpandElement.png)
	
     
   
   - Click "Generic On Off Server"
   
     ![GenOnOffServer](../images/mesh/GenOnOffServer.png)
   
     
   
   - Click "BIND KEY"
   
     ![BindKey](../images/mesh/BindKey.png)
   
     
   
   - Click "Application Key 1"
   
     ![ClickKey](../images/mesh/ClickKey.png)
   
     
   
   - Bound App Key shown below
   
     ![BindedKey](../images/mesh/BoundKey.png)
   
     
   
   - Next setup subscription, by clicking "SUBSCRIBE". We will create and subscribe to a group called "Building 1". This group will be used to turn LED1 on/off on all 3 nodes.
   
     ![Susbcribe](../images/mesh/Susbcribe.png)
   
     ![SubsribeOrginalName](../images/mesh/SubsribeOrginalName.png)
   
     ![SubscribeRename](../images/mesh/SubscribeRename.png)
   
     
   
   - BL653 node is now fully configured and is shown below. Next click "ADD NODE" and repeat above steps to provision and configure the other 2 nodes.
   
     ![BL653SetupComplete](../images/mesh/BL653SetupComplete.png)
   
     
   
   - After provisioning all 3 nodes, navigate to the home screen of nRF Mesh app and select "Groups" and then click "Building 1". NOTE: nRF Mesh app must be connected to one of the nodes.
   
     ![Bldg1Group](../images/mesh/Bldg1Group.png)
   
     
   
   - Toggle LEDs on/off by clicking "ON" or "OFF". You should see LED1 on all nodes go on/off.
   
     ![OnOffBldg1](../images/mesh/OnOffBldg1.png)
   
     

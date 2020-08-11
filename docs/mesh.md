# Getting Started with Mesh

This tutorial will demonstrate how to bring up a Generic OnOff Model Server nodes on the BL65x dev kit; so that we can turn LED1 on/off on the dev kit. We will provision 3 nodes into the network and show you how the message to turn LED on/off propagates in the network.

The goal of this tutorial is to give you a simple example that can get you started on mesh right away. For a deeper dive into Bluetooth Mesh,  refer to the [Bluetooth Mesh Developer Study Guide](https://www.bluetooth.com/blog/bluetooth-mesh-developer-study-guide-v2-0/). 



1. Prerequisites

   - You have followed our [Zephyr Getting Started Guide](ubuntu.md).

   - Install [nRF Mesh](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Mesh) into an Android or iOS device. This will be used to provision the nodes and to turn on/off LED1 on the dev kit

   - You have 3 dev BL65x dev kits. For this demo, we will use a BL562 (node1), BL653 (node2) and BL654 (node3).

     

2. Setup

   

3. Build the Mesh Sample app

   1. Replace main.c from ../samples/bluetooth/mesh/src/main.c with this [main.c](../src/mesh/main.c)

      This replacement main.c is basically the original main.c with the following modifications:

      - Added code to control LED1 of the dev kit. Code is based on the Blinky example

      - Added UUID. NOTE: We will program each node with a different UUID

      - Added code to handle an unacknowledged on/off message
      - Enabled relay function

      It's recommend that you make a backup of the original main.c; so that you can diff it against the replacement main.c. This will allow you to easily see the changes that were done.

   2. Open main.c with a text editor and confirm that the UUID is set for node 1

      ```c
      static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x01 }; //use for node 1
      //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x02 }; //use for node 2
      //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x03 }; //use for node 3
      ```

      

   3. Build and flash node 1

      For this demo we will use the BL652 for node 1

      - To build for BL652:

        ```
        cd ~/zephyrproject/zephyr
        west build -p auto -b bl652_dvk samples/bluetooth/mesh 
        ```

      - To flash

        ```
        west flash
        ```

        

   4. Build and flash for node 2

      For this demo will use the BL653 for node 2

      - Modify main.c to use the node 2 UUID

        ```c
        //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x01 }; //use for node 1
        static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x02 }; //use for node 2
        //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x03 }; //use for node 3
        ```

        

      - To build for BL653

        ```
        cd ~/zephyrproject/zephyr
        west build -p auto -b bl653_dvk samples/bluetooth/mesh 
        ```

      - To flash

        ```
        west flash
        ```

   5. Build and flash for node 3

      For this demo will use the BL654 for node 3

      - Modify main.c to use the node 3 UUID

        ```c
        //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x01 }; //use for node 1
        //static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x02 }; //use for node 2
        static const uint8_t dev_uuid[16] = { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x03 }; //use for node 3
        ```

        

      - To build for BL654

        ```
        cd ~/zephyrproject/zephyr
        west build -p auto -b bl654_dvk samples/bluetooth/mesh 
        ```

      - To flash

        ```
        nrfjprog -f nrf52 --eraseall
        west flash
        ```

   

4. Provision nodes, enable Generic On Off Model  and setup a Subscription

   - Power up all 3 nodes
   
   - Optional: If you are powering the dev kit via the USB1 port, you can monitor debug messages with a terminal emulator (e.g. Tera Term) set to 115200, N, 8, 1. 
   
   - Launch the nRF Mesh and then click "ADD NODE". You should see the 3 nodes as shown below. To provision, click one of the nodes.
   
     ![3Nodes](../images/mesh/3Nodes.png)
   
     
   
   - Next click "IDENTIFY"
   
     ![AddNode1](../images/mesh/AddNode1.png)
   
   - Next click "PROVISION"
   
     ![Provision](../images/mesh/Provision.png)
   
   - Select No OOB
   
     ![OOB](../images/mesh/OOB.png)
   
   - Provision Complete
   
     ![ProvisionComplete](../images/mesh/ProvisionComplete.png)
   
   - Click the provisioned node
   
     ![ClickProvsionedNode](../images/mesh/ClickProvsionedNode.png)
   
   - Click node name to rename
   
     ![NodeName](../images/mesh/NodeName.png)
   
     
   
     ![OldName](../images/mesh/OldName.png)
   
     
   
     ![NewName](../images/mesh/NewName.png)
   
     
   
   - On Off Server
   
     ![ExpandElement](../images/mesh/ExpandElement.png)
   
     ![GenOnOffServer](../images/mesh/GenOnOffServer.png)
   
     ![BindKey](../images/mesh/BindKey.png)
   
     ![ClickKey](../images/mesh/ClickKey.png)
   
     ![BindedKey](../images/mesh/BindedKey.png)
   
     
   
   - Subscriber
   
     ![Susbcribe](../images/mesh/Susbcribe.png)
   
     ![SubsribeOrginalName](../images/mesh/SubsribeOrginalName.png)
   
     ![SubscribeRename](../images/mesh/SubscribeRename.png)
   
     Repeat above for the 2 other nodes
   
     ![BL653SetupComplete](C:\GitHub\Test\images\mesh\BL653SetupComplete.png)
   
   - Repeat above for the 2 other nodes
   
   - After provisioning all three, go back to home screen.
   
   - a
   
   - a
   
   - a



5. 

6. sss

7. Flash the build

   - Connect PC to USB2 port of the dev kit

   - Flash the build

     ```
     west flash
     ```

     After flashing, the module should automatically go into advertising mode.

     

8. Connect to the module using nRF Toolbox

   - 


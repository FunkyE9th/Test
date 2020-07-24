# Enabling the 32kHz Crystal on the BL65x Dev Kit

For designs that require the least amount of current consumption, it's recommended that you use an external +/- 20ppm 32.768kHz crystal. Laird's BL65x dev kit come with the 32.768kHz crystal. However, the default setting is not connected. This tutorial will show you how to connect the 32.768 kHz and  build the Health Thermometer sample app with the 32kHz crystal connected.



**Prerequisites:**

- You have followed our [Getting Started Guide](ubuntu.md).

- Install nRF Toolbox into an Android or iOS device.



Steps

1. Connect the 32.768kHz Crystal

   - Unsolder R127 and R128 from the dev kit

   - Short Solder Bridge SB8 and SB9

     ![](../images/SolderBridge.PNG)

     

2. 


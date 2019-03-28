# Contra BLE Keyboard tutorial 

This tutorial explains how to convert a normal [Contra keyboard](https://cartel.ltd/projects/contra/) into a Bluetooth keyboard. 

I've started with a blanked kit, but if you already built it and are willing to desolder the Pro Micro and all the switches, this is the right tutorial for you!

## List of materials 

* [Sparkfun Pro Micro - 3.3V/8MHz](https://www.sparkfun.com/products/12587) - We need to switch from the 5v Pro Micro that comes on your kit to a 3.3v Pro Micro to make the keyboard more energy efficient and suitable to run on a battery.
* [Adafruit Bluefruit LE SPI Friend - Bluetooth Low Energy (BLE)](https://www.adafruit.com/product/2633) - This BLE module is well supported by the QMK firmware.
* [Adafruit LiIon/LiPoly Backpack Add-On for Pro Trinket/ItsyBitsy](https://www.adafruit.com/product/2124) - A tiny module that will charge the battery when the keyboard is connected through USB.
* [Eachine EV800 FPV Goggles Spare Part 3.7V 2000mAh Lipo Battery](https://www.banggood.com/Eachine-EV800-FPV-Goggles-Spare-Part-3_7V-2000mAh-Lipo-Battery-p-1072727.html) - A nice and compact battery that fits on the 3D printed support. You can use whatever Lipo battery you like as long as it fits on the support.
* [Breadboard-friendly SPDT Slide Switch](https://www.adafruit.com/product/805) - I've used this switch to modulate the support, but you can also use any switch as long as it fits on the support.
* 1N5817 Schottky diode - We need a diode to prevent the Vcc current (500mA) from flowing into the battery when charging the keyboard.
* 30AWG Wire - To connect the components and solder into the MEGA32U4 pads directly.
* Hot glue 
* Labels 
* Electrical tape 
* 3x 2mm diameter washers 
* 3x 6mm heigh standoffs 
* 3x M2*6mm head hex socket screws - I used this to screw the printed support 
* Access to a 3D Printer - To print the [components support](https://www.thingiverse.com/thing:3516520)

## The plan 
Since the Arduino does not have enough pins available to connect the Bluetooth module, we will need to hack and reroute the following pin directly from the microcontroller chip:

![Wiring](images/wires.png)

Here's the package wiring in detail:

<img src="images/package.png" width="500">

We are also going to use the `RAW` Pin (`5v`) from the Arduino to charge the battery. 

The `VCC` Pin (`3.3v`) will have two roles - To supply power to the Adafruit Bluefruit when charging through USB and to receive power from the battery when the keyboard is wireless.

## Building process
First I suggest labeling every wire connection to keep track of what is what. Things will get messy in no time. 

**Step 0.**
Download the STL file of the components support from Thingiverse ([link](https://www.thingiverse.com/thing:3516520)) and print it.

You can print it in any colour or material as long as it's hard enough to support and glue the modules. I recommend PLA since it's easy to work with.

**Step 1.**
Follow the Contra's [assembly guide](https://cartel.ltd/projects/contra/#assembly) until step 3.

**Step 2.**
Remove the 2 headers from the `PIN 14` and `PIN 16` pins that are on the right headers' row, and solder some 30AWG wires in their place, like so: 

![Headers](images/headers.png) 

Label this wires `PIN 14` and `PIN 16` from top to bottom. It will be easier to identify which wire is which.
 
**Step 3.**
Follow the Contra's [assembly guide](https://cartel.ltd/projects/contra/#assembly) until step 7B.

**Step 4.**
This is the *pièce de résistance*! We will solder every wire into the Pro Micro.

**Step 4A.** 
Start by soldering the `PIN 14` and `PIN 16` wires from **Step 2.** into the MEGA32U4's pads.

**Step 4B.** 
After this, solder some 30AWG wires to the `IRQ`, `CS` and `RST` pads. Suggestion - Tape each wire to the keyboard after soldering, and label them. **Be careful, these connections are really delicate!**

**Step 4C.** 
Solder the remaining `5v`, `GND`, `3.3v`, `CLK`, `MISO` and `MOSI`. For the Power and GND pins solder Red and Black wires respectively, following the convention. Don't forget to label them as well.

You should end with something like this:

![Wires from Arduino](images/wires-from-arduino.png) 

**Step 4D.** 
Put some hot glue on the chip connections so that they won't fall off. They are really delicate!

**Step 5.** 
Let's move into something easier. Prepare the ON/OFF switch by soldering two wires into its pins, and cutting the extra pin. You won't need it.

![Switch](images/switch.png) 

**Step 6.** 
Prepare the LiPoly Backpack by adding a Schottky diode on its `VBAT` terminal. **Note the orientation of the diode!!! The strip on the diode should not be facing the module. If backward this will NOT work and you'll probably damage your components**. 

Add wires to the end of the diode and to the `GND` pin, like the image shows.   

![LiPoly Backpack](images/backpack.png) 

**Step 6A.** 
Cut the trace that allows the ON/OFF functionality on this module. You can also solder the 500 mA charging rate, it's optional but I recommend it. For more info, you can have a look at [Adafruit's Documentation](https://learn.adafruit.com/adafruit-pro-trinket-lipoly-slash-liion-backpack/pinouts)

**Step 7.** 
To prevent any short circuit, place electrical tape on this spot, the LiPoly Backpack will be on top of it.

![Eletrical Tape](images/electrical-tape.png)

**Step 8.** 
Grab your 3d printed support and add it the power switch and the LiPo backpack. Screw it on the keyboard with washers between standoffs and solder the switch wires into the battery module. Like so:

![Support In](images/screw-support.png)

**Step 9.**
Now it's time to solder the wires into the Bluefruit module. You'll be passing every wire through the support's hole. Starting by the power wires (`5v`, `GND`, `3.3v`) and ending with the communication wires (`RST`, `IQR`, `CS`, `MOSI`, `MISO`, `CLK`). The result should be something like this:

![Connections](images/connections.png)

**Step 9A.**
First, the `5v` wire goes straight to the LiPoly Backpack 5v terminal:

![5v In](images/5v.png)

**Step 9B.**
After this try to manage the `GND` and `VBAT` wires from the battery module a bit. By routing them on the back of the BLE module (you can twist them to keep them in place).

![Cable Management](images/cable-management.png)

**Step 9C.**
With the module in place, solder the `3.3v` wire from the Arduino into the VIN's BLE terminal.

The `GND` wire that also comes from the Arduino should be solder on the BLE's terminal. 

Connecting the battery module `GND` and `VBAT` in the process.

![GND and 3.3v In](images/power-wires.png)

**Step 9D.**
Now solder, the remaining communication wires into the BLE module by the following order: `RST`, `IQR`, `CS`, `MOSI`, `MISO`, `CLK` (the `SCK` on the module is the `CLK`)

![Communication wires In](images/communication-wires.png)

**Step 10.**
Make sure that everything is well soldered. Glue the module and the cables to keep things tighter. Connect the battery. 
Flash the firmware and...

![Final Result](images/final-result.png)

<h3>Enjoy your wireless Contra!!!</h3>

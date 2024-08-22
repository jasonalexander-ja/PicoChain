# PicoChain

PicoChain is a simple standard for creating daisy chains of connected devices, allowing I2C and power to be easily distributed out using ethernet patch cables. 

This is a repository containing documentation and reference hardware + software implemtations. 

All hardware reference implmentations are in KiCAD 8+ format with the PCB layoud and gerbers. 

### Contents: 

- [PicoChain](/PicoChain); The reference device using the Raspberry Pi Pico (RP2040) as a device. 
- [PicoChainATtiny85](/PicoChainATtiny85); Another reference design using the ATtiny85 and breaking out a pin header for a servo. 
- [PicoChainReference](/PicoChainReference); A generic reference design breaking out the power and I2C to pin headers, this can be easily adapted to what ever purpose is desired. 
- [PicoChainZero](/PicoChainZero); A reference design using the Raspberry Pi Zero as a I2C controller, breaks out the Pi Zero's serial to a USB-C connector. 


## Design Overview 

PicoChain uses differential I2C, 12 + 24 volts, all sent down a single RJ45 & twisted pair allowing communication and power for microcontrollers + motors to be supplied using commonly available ethernet patch cables. 

Generation of the differential I2C is handled by the [NXP PCA9615](https://www.nxp.com/docs/en/data-sheet/PCA9615.pdf) meaning that when SCL and SCA is hooked up to this chip, all the same I2C libraries and implmentation automatically work with no further modifications. 

The 12 + 24 volts is supplied with the assumption that this will be regulated into the relevant required voltages by the connected devices, the reference implementations regulates a 5V for the microcontroller and a 5V for servo and similar small motors off of the 12V line, and a 12V line is regulated off of the 24V line intended for high power motors. 

Each PicoChain device has 2 RJ45 connectors to send and receive power and data, they don't have a particular direction are all the reference designs have the RJ45s passively connected, tapping off of power and I2C as needed, however, all PicoChain chains need terminators at either end. 

Generally, this is designed for running sequences accross some connected devices, with the idea that only 1 large load (I.E. a motor) will be running at a time, reducing the current load going through the patch cables and chained devices. 

### Pinout 

| **RJ45 Pin** | **Function** |
|--------------|--------------|
| **1**        | GND          |
| **2**        | +24V         |
| **3**        | DSCLM        |
| **4**        | DSCLP        |
| **5**        | DSDAP        |
| **6**        | DSDAM        |
| **7**        | GND          |
| **8**        | +12V         |


Pins 3-6 (inclusive) are generated my the [PCA9615](https://www.nxp.com/docs/en/data-sheet/PCA9615.pdf) and are connected directly to ICs on each device. 


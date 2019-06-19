# National-Instruments-FSL-0010
Python class to control the NI Quicksyn FSL-0010 Microwave Synthesizer

image2.jpeg

Find the programming manual and datasheet for this device under Manuals and Datasheets on the Google drive, or on the National Instruments website.
Features

This device may be controlled with Python through its USB interface.

Similar to the NI FSW-0020 Synthesizer, this device produces microwave frequency signals with high precision. However, it offers fewer features and a reduced range.

It will output frequencies between 500 Mhz and 10 GHz with 0.001 Hz precision, at a fixed power of +15 dBm.

It can sweep those frequencies with switching time of 100 us, and dwell times up to 4294s (about one hour). It also offers “list mode” (also called “table mode”) which allows a user to set up a list of frequencies and dwell times, which the device will run through on command.

All sweeps may be run with a software trigger, a hardware trigger, or repeated hardware triggers at each point.
Hardware
image3.jpeg

This device has three main ports: SPI (for power and triggering), USB (interface), and RF out (signal output). Use the provided SPI connector and cable to connect to the SPI port, a USB cable for the USB port, and a SMA connector for RF OUT.

For hardware triggering, attach a trigger wire to the SPI cable, and ground it to the REF IN connector. Hardware trigger signals must be 3.3 V, about 1 mA, and should last less than 1 second.

To prevent damage, this device must not reach 55 °C. The attached heat sink should prevent this, but be sure to use a computer fan or other additional cooling when using this device for longer than 30 minutes.

Find our Python class for this device on the Google Drive under code/Samarium_control/Widgets/Objects. It is listed as “quicksyn_synthesizer.py”.
Software

To set up this device with Python, type find /dev/ttyACM* into terminal before and after plugging in the USB cable. Once you know the device’s address, fill it into the “device_address” parameter in the init function.

To test any command, first run the file. It will initialize the synthesizer object “fg”. Type print(fg.<command>) to run an arbitrary <command> and see the device’s output (or that of our code). We recommend always printing the return values for all commands for debugging purposes.

The basic commands for this device allow a user to get and set output frequencies, turn the output on and off, get the output status, get the device’s temperature (in degrees Celsius) and get the device’s ID (serial number and related info). These are all intuitive and well documented within our code.

To set sweeps between two frequencies, there are two similar commands. Setting a normal sweep means choosing step frequency (which must divide the frequency range). Setting a fast sweep means choosing the number of points on the sweep. Each may be triggered with software, hardware, or run point by point with hardware. As well, each allows for a universal dwell time, sent in ms.

To use list mode, set each point of the list individually using the “list_point_setup” command. You may overwrite points and modify points in any order, specifying a frequency and dwell time for each. Lists require a separate “run list” command. Note that specifying a dwell time other than zero will apply a universal dwell time to the points the the list. Also, use the erase list command to clear any saved sweeps before setting a new one.

The sweep, list, and get/set output commands are sent in hex due to a firmware issue, which NI has acknowledged and we have decided not to fix.

# RPi-iOS Backup Machine (RIBM)
Use a Raspberry Pi Zero to automatically backup your iOS device locally, cloud-free.
This device uses a Raspberry Pi Zero (1 or 2), a custom circuit board and a battery to automatically backup your entire iOS device during charging.
It features:
 - A 650mAh lithium battery to avoid shutting down brutally the RPi Zero when unplugged from the power, therefore extending the lifetime of the microSD card
 - Default Apple backup encryption (supposedly very strong)
 - A tiny OLED display to show the progress of the backup in real time
 - A 2.1A power module that charges the battery, the iOS device and the RPi Zero
 - USB type-C ports to power the device and to connect the iOS device

Tested with iOS 17.4.1 on iPhone 14.

# Hardware components
## Enclosure / Case
The case is optimized for 3D printing without any special configuration. It is comprised of two pieces: the container and a lid.
Bill of materials:
 - 4x M2.5 threaded inserts
 - 4x M2.5 screws
 - 4x M2.5 nylon standoffs
 - Epoxy resin or cyanoacrylate glue

![immagine](https://github.com/giovi321/rpi-ios-backup-machine/assets/6443515/9f952f51-1de0-4623-a86d-08f3c7d90ea5)

## Electronics
The device features a custom designed circuit with 4 mofsets, a voltage divider and a lithium battery charger module.
Evertything connects to the circuit, that is placed on top of the RPi Zero fitting perfectly.
How it works: power management:
 - When the RIBM is plugged to power, the first two MOSFETs give power to the circuit turning on the RPi and one GPIO pin of the RPi will be set to HIGH to signal to the RPi that the device is connected to power
 - When the RPi completes boot, it sets one GPIO HIGH, turning on the other two MOSFETs
 - When the RIBM is disconnected from the power, the first two mosfets turn off, but the second pair of MOSFETs are kept on by the RPi GPIO pin, the whole device is kept alive by the small lithium battery
 - As soon as the power is disconnected, one GPIO pin of the RPi is LOW, signaling to the RPi that it is time to kill the backup process and shut down the RPi

Bill of materials:
 - 2x IRFR9024 MOSFET
 - 2x 2N7000 MOSFET
 - 2x 100k resistor
 - 2x 10k resistor
 - 1x xxx resistor (voltage divider)
 - 1x yyy resistor (voltage divider)
 - 2x 20 PIN female header
 - Some thin wire
 - 2x female USB plugs (one of them needs a SMD resistor to enable powering via USB-C power supply that provides more than 5V)
 - Circuit board
 - 0.91" 128x32 i2c OLED display
 - Raspberry Pi Zero / Raspberry Pi Zero 2
 - MicroSD card large enough to store a full backup of your iOS device

![immagine](https://github.com/giovi321/rpi-ios-backup-machine/assets/6443515/2c802494-e9c2-49bf-b19f-a2cdf4a6c33f)


# Software
The backup is performed using libimoibledevice's idevicebackup2.
The backup process and the display are all managed by a simple python script.
The GPIO signals 

How it works - back-up process:
 - As soon as an iOS device is connected the RIBM checks if it is a known device, if yes, it starts the backup process
 - On the display, simple text will appear telling what's going on until the backup actually begins
 - At this point there will be a percentage of backup completion and a simple animation that, if is moving, indicates that the backup is running; if it is not moving, it indicates that the backup is stuck
 - There is certain error handling through text shown on the display
 - At the end of the backup there is a text stating the timestamp of the end of the backup

# Improvements for the next version
There are obviously several software improvements, but let's stick to the hardware for now:
 - Use spring connectors to connect the test-pads of the RPi to connect the USB data lines (requires quite an overhaul of the PCB design as the female header has to be moved to the other face of the PCB)
 - Use connectors for battery, display, etc. (coudn't find anything small enough not to affect the form factor of the RIBM
 - Move the battery out of the way of the heat dissipation of the RPi, requires slim header connectors to connect the RPi and the PCB and a slight redesign of the enclosure

# Building from Sources under linux

### Step1: get prerequisites

 - sudo apt install arduino
 - pip install pymavlink

### Step2: get code

 - cd ~
 - git clone https://github.com/ardupilot/arduremoteid
 - cd arduremoteid/
 - git submodule init
 - git submodule update --recursive
 - ./scripts/install_build_env.sh
 - ./scripts/regen_headers.sh
 - ./scripts/add_libraries.sh

## Building with make and arduino-cli

### Step1: Use make to install ESP32 support

 - cd RemoteIDModule
 - make setup

### Step2: Use make to build

 - cd RemoteIDModule
 - make

To build specifically for the ESP32-WROOM-32E target use:

 - make esp32-wroom-32e

To build specifically for the ESP32-C3 0.42 OLED target use:

 - make esp32-c3-042-oled

### Step3: Use make to upload

 - cd RemoteIDModule
 - make upload

To upload the ESP32-WROOM-32E image specifically use:

 - make upload-ESP32_WROOM_32E

To upload the ESP32-C3 0.42 OLED image specifically use:

 - make upload-ESP32_C3_042_OLED

If the board does not flash, hold-down the BOOT pushbutton on the PCB while pressing the RESET pushbutton briefly [to force it into bootloader mode] and retry.
The ESP32-S3 is now running and emitting test/demo remote-id bluetooth

If you get an error about missing serial support from python, install it with `python -m pip install pyserial`.

### Optional

Plugin your ep32-s3 with ANOTHER usb cable using the port labeled
"UART" on the pcb - this is where mavlink and debug is coming/going,
and you can connect to this with mavproxy, etc.

Plugin your ep32-s3 into a flight-controller UART using pins
RX(17)/TX(18)/GND on the pcb - this listens for mavlink from an
autopilot, and expects to find REMOTE_ID* packets in the mavlink
stream, and it broadcast/s this information from the drone as
bluetooth/wifi on 2.4ghz in a manner that can be received by Android
mobile phone App
[https://play.google.com/store/apps/details?id=org.opendroneid.android_osm]
and hopefully other open-drone-id compliant receivers.

Plugin your ep32-s3 into a flight-controller CAN port by wiring a standard CAN Tranciever (such as VP231 or similar) to pins 47(tx),38(rx),GND on the pcb.

For the ESP32-WROOM-32E target, based on the Adafruit AirLift breakout, the default MAVLink UART is RXI(GPIO3)/TXO(GPIO1)/GND.
No default CAN pins are configured for this target.

For the ESP32-C3 0.42 OLED target, the default MAVLink UART is RX(20)/TX(21)/GND. The onboard LED is on GPIO 8 and is active low. The onboard OLED is wired to I2C on GPIO 5/6 and is not currently used by this firmware.

Setup/Configuration of ArduPilot/Mavlink/CAN to communicate together is not documented here, please go to ArduPilot wiki for more, eg: https://ardupilot.org/copter/docs/common-remoteid.html

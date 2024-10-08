=============
T-DECK
=============

The `T-DECK <https://www.lilygo.cc/products/t-deck-plus>`_ is a small-sized handheld from LillyGo featuring the ESP32-S3 CPU.

.. list-table::
   :align: center

   * - .. figure:: esp32s3_t-deck.jpg
          :align: center

Features
========

  - ESP32-S3 WROOM-1 Module
  - USB-C USB port
  - LCD Display
  - MEMS Microphone
  - Keyboard
  - Trackball
  - LORA Radio
  - GPS Module
  - SD Card Slot
  - 8MB Octal PSRAM
  - 8MB SPI Flash
  - RST and BOOT buttons (BOOT accessible to user)

Configurations
==============
.. todo:: To be updated
.. All of the configurations presented below can be tested by running the following commands::

      $ ./tools/configure.sh esp32s3-eye:<config_name>
      $ make flash ESPTOOL_PORT=/dev/ttyUSB0 -j

  Where <config_name> is the name of board configuration you want to use, i.e.: nsh, buttons, wifi...
  Then use a serial console terminal like ``picocom`` configured to 115200 8N1.

  nsh
  ---

  Basic NuttShell configuration (console enabled in USB JTAG SERIAL Device, exposed via
  USB connection at 9600 bps).

  usbnsh
  ------

  Basic NuttShell configuration console enabled over USB Device (USB CDC/ACM).

  Before using this configuration, please confirm that your computer detected
  that USB JTAG/serial interface used to flash the board::

    usb 3-5.2.3: New USB device strings: Mfr=1, Product=2, SerialNumber=3
    usb 3-5.2.3: Product: USB JTAG/serial debug unit
    usb 3-5.2.3: Manufacturer: Espressif
    usb 3-5.2.3: SerialNumber: XX:XX:XX:XX:XX:XX
    cdc_acm 3-5.2.3:1.0: ttyACM0: USB ACM device

  Then you can run the configuration and compilation procedure::

    $ ./tools/configure.sh esp32s3-eye:usbnsh
    $ make flash ESPTOOL_PORT=/dev/ttyACM0 -j8

  Then run the minicom configured to /dev/ttyACM0 115200 8n1 and
  press <ENTER> three times to force the nsh to show up::

    NuttShell (NSH) NuttX-12.1.0
    nsh> ?
    help usage:  help [-v] [<cmd>]

        .         break     dd        exit      ls        ps        source    umount
        [         cat       df        false     mkdir     pwd       test      unset
        ?         cd        dmesg     free      mkrd      rm        time      uptime
        alias     cp        echo      help      mount     rmdir     true      usleep
        unalias   cmp       env       hexdump   mv        set       truncate  xd
        basename  dirname   exec      kill      printf    sleep     uname

    Builtin Apps:
        nsh  sh
    nsh> uname -a
    NuttX 12.1.0 38a73cd970 Jun 18 2023 16:58:46 xtensa esp32s3-eye
    nsh>

Flashing
========

Because T-Deck doesn't use an external USB/Serial chip like others ESP32
boards you should put it in programming mode this way:

  1) Press and hold BOOT(Trackball) and RESET (Side) buttons at same time;
  2) Release the RESET button and keep BOOT button pressed;
  3) After one or more seconds release the BOOT button;
  4) Run the flashing command: make flash ESPTOOL_PORT=/dev/ttyACM0

Serial Console
==============

The internal USB JTAG SERIAL Device, by default, is used as serial console.
It is normally detected by Linux host as a USB CDC/ACM serial device.

It will show up as /dev/ttyACM[n] where [n] will probably be 0.

You can use minicom with /dev/ttyACM0 port at 9600 8n1 or picocom this way:

  $ picocom -b9600 /dev/ttyACM0

Peripherals
================

Board Buttons
-------------
There are two physical buttons BOOT(Trackball) and RST(Side).
The board also has a power-on slider.
The Trackball has one physical button and 4 magnetic switches used to sense the roll of the ball.

===== ========== ==========
Pin   Signal     Notes
===== ========== ==========
IO00  BOOT       Trackball button
RST   RST        Reset button
IO02  UP         Trackball up
IO01  LEFT       Trackball left
IO15  DOWN       Trackball down
IO03  RIGHT      Trackball right
===== ========== ==========

Keyboard
-------------
The keyboard is internally connected to the ESP32-C3 microcontroller. It has preuploaded firmware for accessing the keyboard over i2c. The ESP32-C3 also controls the keyboard backlight

===== ========== ==========
Pin   Signal     Notes
===== ========== ==========
IO18  SDA        I2C SDA
IO8   SCL        I2C SCL
IO46  INT        Keyboard interrupt  
===== ========== ==========

LCD Display and Touchscreen
-------------
The LCD display is connected to the ESP32-S3 microcontroller over SPI.
The LCD display is a 2.8 inch 320x240 pixel display with a ST7789 controller.
The Touchscreen is connected to the ESP32-S3 microcontroller over I2C and uses a GT911 controller.
===== ========== ==========
Pin   Signal     Notes
===== ========== ==========
IO41  MOSI       SPI MOSI
IO38  MISO       SPI MISO
IO40  SCK        SPI SCK
IO11  LCD DC     LCD Data/Command
IO12  LCD CS     LCD Chip Select
IO42  BCKL       Backlight Enable
IO18  SDA        I2C SDA
IO8   SCL        I2C SCL
IO16  INT        Touchscreen interrupt  
===== ========== ==========

Speaker and Microphone
-------------
.. todo:: To be updated
There are two buttons labeled BOOT and RST.

===== ========== ==========
Pin   Signal     Notes
===== ========== ==========
?     ?          ?
===== ========== ==========

SD Card
-------------
.. todo:: To be updated
There are two buttons labeled BOOT and RST.

===== ========== ==========
Pin   Signal     Notes
===== ========== ==========
?     ?          ?
===== ========== ==========

LORA Radio
-------------
.. todo:: To be updated
There are two buttons labeled BOOT and RST.

===== ========== ==========
Pin   Signal     Notes
===== ========== ==========
?     ?          ?
===== ========== ==========

Other
-------------
.. todo:: Confirm the information below
The board has a resistor divider for measuring the battery voltage and a Power On pin.
To keep the ESP32 from turning off keep the Power On pin high.

===== ========== ==========
Pin   Signal     Notes
===== ========== ==========
IO4   BAT_ADC    Multiply measured voltage by 2 to get battery voltage
IO10  PWR_ON     Keep high to keep the ESP32 from turning off
===== ========== ==========

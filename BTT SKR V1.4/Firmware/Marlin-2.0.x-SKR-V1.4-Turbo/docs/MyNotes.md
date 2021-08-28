# Notes


Marlin\src\core\boards.h confirms i have LPC1769  not 1758


## setup 


!!! warning Instructions appear well out of date.

distilled from the docs from mfg

        After opening the project, go to the platformio.ini file and change
        the default environment from megaatmega2560 to LPC1768,
        env_default = LPC1768

Used LPC1789 -


>Then go to the configuration.h file and modify it

>           #define SERIAL_PORT -1
>           #define SERIAL_PORT_2 0
>           #define BAUDRATE 115200
>           #define MOTHERBOARD BOARD_BIQU_SKR_V1_1


> After the compilation is successful, a **firmware.bin** file will be generated in the 
`.pioenvs\LPC1768` directory. We will copy this file to the TF card of the motherboard, and then reset the motherboard, so that the firmware is burned into the motherboard.


Marlin\src\HAL\LPC1768 has the location of the windows driver

[Marlin\src\HAL\LPC1768\win_usb_driver\lpc176x_usb_driver.inf](Marlin\src\HAL\LPC1768\win_usb_driver\lpc176x_usb_driver.inf) is the driver (link does nothing.)


!!! warning From 1.3
    Modify the following lines:
        //#define BLTOUCH and change to #define BLTOUCH
        ok
        //#define AUTO_BED_LEVELING_BILINEAR and change to #define AUTO_BED_LEVELING_BILINEAR
        ok
        // #define Z_SAFE_HOMING and change to #define Z_SAFE_HOMING
        ok
        // #define ENCODER_PULSES_PER_STEP 4 and change to #define ENCODER_PULSES_PER_STEP 4
        // #define REVERSE_ENCODER_DIRECTION and change to #define REVERSE_ENCODER_DIRECTION
        //#define REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER and change to #define REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER
        If you like to activate Babystepping switch to Configuration_adv.h
        //#define BABYSTEPPING and change to #define BABYSTEPPING
        BIGTREETECH SKR V1.3 guide 2019 – V6 by Jupa Creations Page 7
        //#define DOUBLECLICK_FOR_Z_BABYSTEPPING and change to #define DOUBLECLICK_FOR_Z_BABYSTEPPING
        //#define BABYSTEP_ZPROBE_OFFSET and change to #define BABYSTEP_ZPROBE_OFFSET
        //#define BABYSTEP_ZPROBE_GFX_OVERLAY and change to #define BABYSTEP_ZPROBE_GFX_OVERLAY
        #define BABYSTEP_MULTIPLICATOR 1 and change to #define BABYSTEP_MULTIPLICATOR 20
        If your original BL-touch, clone TL-touch or clone BT-touch is dropping the pin during printing switch to Marlin/scr/inc/conditionals_LCD.h and change the following code:
        #define BLTOUCH_STOW 100 // was 90 #define BLTOUCH_SELFTEST 130 // was 120

Fan setup

this connects pronterface


RasPi can connect to the TFT port

L->R

RESET | RX | TX | GND | NPWR 

connect gnd, RX (pi pin 10) and TX (pi pin8) to the TFT port


## drivers 

2209 has stallguard.  2208 does not.

SPI mode used in TMC2130, TMC5160, TMC5161

so use uart

E1Det is also used for power backup and resume.



## fans

It seems that only `Fan0` can switch.  `Fan1`,`Fan2`,`Fan3` appear always connected in the schmatic.

board fan/ for stepper drivers, handled off board.

have Extruder Fans and have part fans, but only one switchable fan.


## Pin Changes

see - (Pin Assignment)[Marlin\src\pins\lpc1768\pins_BTT_SKR_V1_4.h]

## multiplexing fans

I think using a SN74HC157 could "multiplex" fans.

Normally when referenced in Configuration_adv.h - they mean to "select" amoung a single fan.  But I would want to be able to control multiple fans.

Really there is no real benefit in multiplexing- 4 inputs, creates 16 states, which controls....4 fans.
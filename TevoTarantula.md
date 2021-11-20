# Tevo Tarantula Notes

## Introduction

JimBrown's EasyConfig is no longer maintained.

The goal of this branch is to incorporate JimBrown's EasyConfig changes for the Tevo Tarantula 
back to a branch on the main Marlin repository.

JimBrown's repo contains changes to the following files:

 * Marlin/src/pins/pins_RAMPS.h
 * Marlin/Configuration.h
 * Marlin/Configuration_adv.h

The most significant change is a new section at the top of Configuration.h where most Tarantula customisations have been
concentrated, through the use of new compiler definitions.

## Summary Of Changes

Board is a Tevo-supplied MKS Gen 1.4 Board, with an ATMega 2560 microcontroller (confirmed by sight).
Stepper drivers are built-in HR4982 (Heroic HR4982, similar to the Allego A4982 but only support 1x, 8x, 16x & 128x microstepping).

Define BOARD_MKS_GEN_13 (includes MKS GEN v1.3 and v1.4, pins_MKS_GEN_13.h).

src/pins.h for MKS_GEN_13 indicates env:mega2560, which is the default target in platformio.ini.

Build with:

```
 $ pio run -e mega2560
 $ ~/.platformio/packages/toolchain-atmelavr/bin/avr-objcopy -O binary -I ihex \
    .pio/build/mega2560/firmware.hex .pio/build/mega2560/firmware.bin
```

 * Set STRING_CONFIG_H_AUTHOR to "David Antliff, TEVO Tarantula Custom Config"
 * Set CUSTOM_MACHINE_NAME to "TEVO Tarantula"
 * Set BAUDRATE to 115200
 * Enable SDSUPPORT - SD card slot support
 * Set MOTHERBOARD to BOARD_MKS_GEN_13
 * Set TEMP_SENSOR_BED from 0 to 1
 * Uncomment and set Z_HOMING_HEIGHT to 5
 * Set DEFAULT_AXIS_STEPS_PER_UNIT array: [2] to 1600 (Z_STEPS), [3] to 402 (calibrated E0_STEPS)
 * Set PROBE_MANUALLY
 * Set INVERT_E1_DIR to true

SENSOR_LEFT... calculation is a little tricky.

Can use git diff later to figure out all changes


## Other

Dump all compile-time definitions from a header file with:

```shell
gcc -E -dM -include Marlin/src/core/macros.h -include Marlin/Configuration.h - < /dev/null | grep -v "define __" | grep -v "define _"
```


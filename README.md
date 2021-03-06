# Automated Indoor Irrigation

# Table of Contents

- [1) Hardware](#1-hardware)
   - [1.1) Hardware Requirements](#11-hardware-requirements)
   - [1.2) Hardware Setup](#12-hardware-setup)
- [2) Software](#2-software)
   - [2.1) Setup Environment](#21-setup-environment)
   - [2.2) Code Customization Options](#22-code-customization-options)
   - [2.3) Running the Digital Calendar and Photo frame](#23-running-the-digital-calendar-and-photo-frame)

# 1) Hardware

## 1.1) Hardware Requirements

- [Any 96Boards CE (We will be using the Drogonboard410c)](https://www.96boards.org/products/ce/)
- [LinkSprite Mezzanine](https://www.96boards.org/product/linkspritesensorkit/)
- 1x [I2C 16x2 LCD Module](https://www.amazon.com/LGDehome-Interface-Adapter-Backlight-MEGA2560/dp/B0711WLVP9)
- 1x [Soil Moisture Sensor LM393](https://www.amazon.com/XCSOURCE-Moisture-Automatic-Watering-TE215/dp/B00ZR3B60I)
- 1x [Motor controller L298n](https://www.amazon.com/Qunqi-2Packs-Controller-Stepper-Arduino/dp/B01M29YK5U)
- 1x [5v Water Pump](https://www.amazon.com/Vipe-Micro-Submersible-Motor-Water/dp/B01N0X3CW4)
- 1x Pipe: Length and diameter as per requirement
- Connecting wires

## 1.2) Hardware Setup

- **Plugin the LinkSprite Mezzanine on the CE Board.**
- **Plug in the LCD scree to the LinkSprite Mezzanine as follows:**

| I2C 16x2 LCD | LinkSprite Mezzanine |
|:------------:|:--------------------:|
| VCC          | IIC VCC              |
| GND          | IIC GND              |
| SDA          | IIC SDA              |
| SCL          | IIC SCL              |

- **Plug in the Soil Moisture Sensor to the LinkSprite Mezzanine as follows:**

| Soil Moisture Sensor | LinkSprite Mezzanine |
|:--------------------:|:--------------------:|
| VCC                  | ADC1 VCC             |
| GND                  | ADC1 GND             |
| A0                   | ADC1 A0              |

- **Plug in the L298n Motor Controller to the LinkSprite Mezzanine as follows:**

| L298n | LinkSprite Mezzanine |
|:-----:|:--------------------:|
| VCC   | D1 VCC               |
| GND   | D1 GND               |
| IN 1  | D1 DA                |
| IN 2  | D1 GND               |

- **Plug in the L298n Motor Controller to the 5v Pump as follows:**

| L298n | Water Pump |
|:-----:|:----------:|
| OUT 1   | +5v      |
| OUT 2   | GND      |

- **Add bury the Probes of the Soil Moisture sensor in the Pot.**

# 2) Software

**This guide assumes that [Debian OS is running on a Dragonboard410c](https://www.96boards.org/documentation/consumer/dragonboard410c/downloads/debian.md.html). How ever the instructions hold true for other 96Boards CE Boards running Debian.**

> This project is compatible with other Linux based OS, but they might have to be tweaked accordingly.


## 2.1) Setup Environment

- **Enable spidev as shown here: [Enabling SPIDEV on Dragonboard410c](https://www.96boards.org/documentation/consumer/dragonboard/dragonboard410c/guides/enable-spi.md.html)**

- **[Build MRAA](https://www.96boards.org/documentation/consumer/guides/mraa/install.md.html)**

- **[Build UPM](https://github.com/intel-iot-devkit/upm/blob/master/docs/building.md)**

## 2.2) Code Customization Options

- **Calibrate Senor**
  - Insert sensor into dry and then wet soil and modify the ```in_min``` and ```in_max``` values here:
    - line 119: ```water_level = map(adc_value, 550, 10, 0, 100);```
  - Do this till dry soil reads 0% and wet soil reads 100%

- **Soil Moisture Percentage**
  - The target moisture percentage can also be modified by editing line 127 and 133.

## 2.3) Running the Digital Calendar and Photo frame

- **Build Project:**
  - ```gcc src/pot.c -o pot -lmraa -lupmc-lcm1602 -lupmc-utilities -I/usr/include/upm```


The LCD screen should now show the current Pump status and Moisture Level in the soil. And the pump should start pumping water once the moisture level drops.

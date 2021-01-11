# Hacking the IKEA TRÅDFRI

* [Introduction](#introduction)
* [TRÅDFRI modules](#tr-dfri-modules)
* [Firmware analysis](#firmware-analysis)
* [Custom firmware](#custom-firmware)
* [Working safely](#working-safely)
* [Test setup](#test-setup)
* [Other sources](#other-sources)
* [License](#license)
* [Disclaimer](#disclaimer)

## Introduction
The [IKEA TRÅDFRI](http://www.ikea.com/us/en/catalog/categories/departments/lighting/36812/)
family of products provide you with several home automation solutions that
interconnect using [ZigBee Light Link](http://www.zigbee.org/zigbee-for-developers/applicationstandards/zigbee-light-link/).
While the line-up initially only included lighting products, it includes power
switches and wireless window blinds as well.

Many of the TRÅDFRI are quite simple. For instance, if we take a simple light
bulb, it contains:

* Power supply
* LED driver
* IKEA TRÅDFRI module

The IKEA TRÅDFRI module is used in many of their products, and is actually
a small piece of circuit board with a few GPIO pins exposed. These pins are
then used to control the LED driver.

You can take out the board, and hook it up to your own lighting solutions. Or,
you can flash it with your [own firmware](#custom-firmware), for other purposes.

To find relevant products, I have compiled a list of IKEA TRÅDFRI products in
[PRODUCTS.md](PRODUCTS.md) (please help me to update this list). Several
products have been opened up. Teardown pictures can be found in the
[teardowns](teardowns/) folder.

## TRÅDFRI modules
So far, a few variations of the TRÅDFRI modules have been identified. They are
all using microcontrollers manufactured by Silicon Labs. The modules that have
been identified are:

* ICC-1
* ICC-A-1
* MGM210L

[<img src="images/icc-1-front.jpg" alt="Front of IKEA TRÅDFRI module (ICC-1)" width="256">](images/icc-1-front.jpg)
[<img src="images/icc-a-1-front.jpg" alt="Front of IKEA TRÅDFRI module (ICC-A-1)" width="256">](images/icc-1-front.jpg)
[<img src="images/mgm210l-front.jpg" alt="Front of IKEA TRÅDFRI module (MGM210L)" width="256">](images/icc-1-front.jpg)

Some other products, such as the line-up of remote controls, have a dedicated
circuit board that integrate a microcontroller directly (i.e. no separate
module board).

More details and pictures on these modules can be found in [MODULES.md](MODULES.md).

## Firmware analysis
An analysis of some firmware versions encountered can be found in
[FIRMWARE.md](FIRMWARE.md).

## Development
The chip is a regular Cortex M4. You can flash it with your own custom
firmware. As a starting point, you could take a look at [this pull request](https://github.com/RIOT-OS/RIOT/pull/8047)
for RIOT-OS.

I've added some firmwares in the [firmwares](firmwares/) folder.

As a proof of concept, check out [this YouTube video](https://www.youtube.com/watch?v=yi_Z2WtmdDU)
I made. In there, I show how I control the LED connected via a serial console.

### EZSP
It is even possible to load the Silicon Labs EmberZNet Zigbee coordinator
firmware on an ICC-1 or ICC-A-1. This allows you to use the module to set-up
your own ZigBee network.

MattWestb has provided a guide and firmware [here](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Module).

## Working safely
If you plan to leave the board in-place, and run your own light bulb firmware,
never connect external devices (e.g. debugger or serial adapter) to a light
bulb that is plugged in. Due to different voltage levels, you could destroy
your devices.

If you want to connect an external device, ensure that it is properly isolated
(e.g. using a optocoupler).

I have designed a board that you could use to isolate UART signals. You can
find it [here](pcbs/isolator).

## Test setup
My setup (the small board is a UART isolator):

[<img src="images/setup.jpg" alt="Test setup" width="256">](images/setup.jpg)

My safer setup, including debugger (LED is connected to same pin as it would in
the GU10 light):

[<img src="images/setup2.jpg" alt="Safer test setup" width="256">](images/setup2.jpg)

Two soldered development boards that I use nowadays:

[<img src="images/setup3.jpg" alt="Safer test setup" width="256">](images/setup3.jpg)

## Other sources
I have gathered some information from the following sources:

* [IKEA Trådfri hacking](https://tradfri.blogspot.nl)
* [Trådfri: ESP8266-Lampen-Gateway](https://www.heise.de/make/artikel/Ikea-Tradfri-Anleitung-fuer-ein-ESP8266-Lampen-Gateway-3598411.html)
* [Trådfri Zigbee Light Link module](https://diystuff.nl/tradfri/tradfri-zigbee-light-link-module)
* [Trådfri Wall switch](https://wiki.iptables.dk/TR%C3%85DFRI_wall_switch)

## License
Creative Commons BY Attribution 4.0 International

## Disclaimer
This page and its content is not affiliated with IKEA of Sweden AB.

The purpose of this project is to learn and improve using reverse engineering
techniques. Use this information on your own risk.

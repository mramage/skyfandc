# SkyFan DC ESPhome Setup

This project is for taking local control over a SkyfanDC ceiling fan made by Ventair, through the use of a custom PCB instead of the TUYA based modules the manufacturer provides. This will allow for local integration with ESPhome and Home Assistant, and will not work with any manufacturer provided apps/platforms (the supplied remote control works though).

This project is a fork of the original project found at https://github.com/jeggleston1981/skyfandc
As of mid-2026 the majority of the code is a direct copy of that original project, with a few modifications I've made to work with more recent versions of ESPhome, as well as adding support for my own board design.

## MRamage ESP32C6 boards
I have created an ESP32C6 based board to replace the original ESP8285 based boards made by James / egglecc. I have based it on the ESP32C6 as it is more modern and that SoC has wider protocol support.

I will be uploading the PCB design files soon (Gerbers, BOM, etc). I have received the first prototype batch, and have it running in a ceiling fan/light since early July 2026. It uses the micro_skyfan_c6.yaml file.

### The ESP32C6 has Zigbee / Thread / Matter - are they supported?
Not yet, but I intend to implement Zigbee for at least the light soon.

One limitation of the wifi-based control is that you are reliant on a functioning home assistant server and wifi network to be running to tie in any controls. I am planning to implement zigbee for at least the light control, to allow for direct binding of a zigbee controller to this board to allow for lights to be turned on/off even if wifi is unavailable or homeassistant is down.

### Can I get hold of one of these boards?
I likely won't be able to manufacture and ship these - I don't want to commit to handling orders and shipping. Instead, I intend to provide all the information and files needed to get these fabricated and assembled through a service such as JLCPCB.

If I find a drop-ship style PCB fabrication service, then I'll consider setting up something though that, but I don't see it being economical for one or two boards at a time. For reference, I paid roughly USD$90 (~AUD$130) for a batch of 5 PCBs to be fabricated and assembled for the first prototype run. If I was then on-shipping, I'd have to add shipping on top of that, plus an amount to cover the time involved to order/program/pack/ship these.

I'm happy for people to make these for others though, as long as it's clear they aren't affiliated with me or this project directly. If someone figures out how to get rich off a niche little PCB I designed, then good for you!

## Roadmap
[x] ESP32C6 based prototype PCB fabricated (completed: July 2026)
[x] Get existing esphome functionlity running on prototype board (completed: July 2026)
[ ] Proof of concept: very basic zigbee implementation of on/off light control functioning on esp32c6 board. Likely to be a standalone test without esphome functionality (planned: July 2026)
[ ] Get zigbee level control of light working (planned: July-Aug 2026)
[ ] Get zigbee light functionality working alongside esphome functionality (planned: July-Aug 2026)
[ ] Test direct binding of IKEA STYRBAR remote control. I believe this will be configured via zigbee2mqtt, but will then make the light respond directly to zigbee transmissions from the remote (planned Aug 2026)
[ ] If all that works, then I may try to get the fan speed control working with the left/right buttons on the remote. From what I have seen those buttons send scene commands, so this is likely going to be a non-standard way of using them and not as reusable for others. If I do get this working, I'll make sure to keep that in a separate file, or have some other way to make it not affect use of this code by others. 

There's a risk that I might not get zigbee and esphome wifi working simultaneously. In that scenario, I will need to decide if zigbee can be done on the esphome platform without wifi enabled, or I may do it on another platform or natively. I will still keep the esphome yaml files for the esp32c6 boards available within this project, and will decide if the zigbee stuff has to be done in a separate project.

NOTE: All estimated timelines are in "Matt Time" which is often not at all aligned with what a normal calendar shows.

## Egglec boards

The basic yaml config for the SkyfanDC made by Ventair flashing it onto your ESP8266 module and put it in the fan controller.
If you would like the physical module it is for ESPhome and Home Assistant only www.egglec.com.au
This module will not work with Tuya Smart App

Video about the fan : https://youtu.be/DethhMjQXy0

EasyEDA files : https://oshwlab.com/james_6977/sky-fan-dc

### Egglec SkyFan DC Module Version 2
If you have version 2 the pinout is different to accommodate the JST SH port that is for I2C, that port is connected to pins 4 and 5 which are the default I2C pins for the ESP8285.  If you have that style board please use the micro_skyfan2.yaml file as a base file to use if you wish to make modifications.


## Features

### Fan

- When a remote control command is received by the fan motor the fan entity is updated.
- The speed and direction of the fan motor can also be controlled by the fan entity.

| Fan Entity Speed | Skyfan DC      |
| ---------------- | -------------- |
| 0.0              | OFF            |
| 0.17             | NORMAL speed 1 |
| 0.33             | ECO            |
| 0.5              | NORMAL speed 2 |
| 0.67             | NORMAL speed 3 |
| 0.83             | NORMAL speed 4 |
| 1.0              | NORMAL speed 5 |

### Timer

Run on Timer with 12 setting options (1hr to 12hrs)

- The Timer duration can be set by the Timer select entity. This entity is disabled by default.

### Modes

ECO, SLEEP and NORMAL

- The Mode can be selected by the Mode select entity. This entity is disabled by default.

#### ECO

Fan will operate at peak energy efficiency level, usually somewhere between speeds 1 and 2.

#### SLEEP

Fan will reduce by 1 speed every 30mins until speed 1. (select preferred starting speed level first)

#### NORMAL

Cancels Mode and returns to normal function.

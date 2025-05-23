# LogiLink-PDU8P01-ESPHome

This repo includes:
- yaml config for the esp, including making use of the power measurement IC
- (Mostly complete) pinout for the header J1 going to the old microcontroller board
- Instructions to rebuild the PDU to use the ESP instead

&nbsp;
&nbsp;

> WARNING: Due to the power distribution unit being mains powered, there is a real risk of electrocution, starting a fire or both.
> I do not take any responsibility should you, someone you love (or even someone you hate), get harmed or killed.

&nbsp;
&nbsp;

The config includes things like:
- Measuring power/current/voltage
- Saving the states of the outputs in case of a power failure and turning them on in a staggered manner
- Ethernet setup for my specific board
- Connection to homeassistant

&nbsp;

I ended up using the Waveshare ESP32 S3 ETH (can be found here https://www.waveshare.com/esp32-s3-eth.htm) for the ESPHome board after a recommendation from a friend.

The reasoning was that it has enough GPIO pins despite also having an Ethernet board attached and supports PoE (which i did not use in my case since the PDU is always powered anyway).
Wi-Fi was not an option due to reliability concerns.

Pinout for J1 (looking straight down at the board with the relais):
| Wire colour left | Pin left | Pin right | Wire colour right |
|:-----------------|:--------:|:---------:|------------------:|
| Black            | 5V       | SEL       | Black             |
| Red              | CF1      | CF        | Red               |
| Blue             | NC       | NC        | Blue              |
| Yellow           | GND      | OUT1      | Yellow            |
| Black            | OUT2     | OUT3      | Black             |
| Green            | OUT4     | OUT5      | Green             |
| White            | OUT6     | OUT7      | White             |
| Red              | OUT8     | GND       | Red               |

What you need/should do:

The reason i converted the PDU in the first place was that the capacitors failed and leaked, killing the switching power regulator and the micocontroller board.
I decided to rip out the transformer "T1", switching mode IC "U3", capacitor "C1" and "C2", the fuse "RF1", capacitor "C5".

Replace the capacitor "C5" with a connector that fits in there, i used a "PTR Hartmann STL 1550" socket and "PTR AK 1550" plug. This will be used by an external 5V 1A USB-brick that you can buy for cheap to feed the 5V for the relais and ESP. (Note that GND (green wire) is on the left of the green connector, +5V (yellow wire) on the right!)
![PXL_20250523_162205871](https://github.com/user-attachments/assets/8cd2f4d9-82ca-4861-b06a-df60ad64fd64)
![PXL_20250523_162229747](https://github.com/user-attachments/assets/d526e1c2-7777-441b-a3e9-53bd97c6b3eb)


I also replaced the capacitors "C4", "C18" and "C20" for piece of mind.

The USB power supply got hard-wired in, connecting the mains leads to after the fuse on one end, the other onto the main power plug of the PDU by soldering. Just be careful so that you can still plug in the wires/connectors after!
![PXL_20250523_165315783](https://github.com/user-attachments/assets/c4e8a836-e959-495a-bdb3-458c0cec52ff) ![PXL_20250523_170831597](https://github.com/user-attachments/assets/1e4013c5-33ee-4d42-b932-bee28d130b12)

This can be done in a better way for sure - i just decided to go the quick & cheap route.

The ESP itself mounts by soldering the ethernet jack onto the PCB of the old miconcontroller board (you will need to clean off some components to get a flat surface on that board.)

![PXL_20250523_172336320](https://github.com/user-attachments/assets/3ebf7a04-6003-4000-89b9-3d43c1884ff8)


To make the connections to the ESP, just cut off the other header, strip all the needed wires a few millimeters (all except the blue ones and the second GND) and solder them directly to the ESP.
![PXL_20250523_161910283](https://github.com/user-attachments/assets/63f6bed2-6ca6-44d4-952d-8b76138f27d5)

The connections are as follows, i tried to avoid all pins that show undefined behaviour while booting:

| Wire from J1 | GPIO | Pin on ESP |
|:-----------------|:--------:|:---------:|
| 5V            | VBUS       | 40       |
| GND              | GND      | 38        |
| OUT1             | GPIO48       | 4        |
| OUT2           | GPIO47      | 5      |
| OUT3            | GPIO16     | 32      |
| OUT4            | GPIO18     | 31      |
| OUT5            | GPIO42     | 9      |
| OUT6              | GPIO41     | 10       |
| OUT7              | GPIO40     | 11       |
| OUT8              | GPIO39     | 12       |
| CF              | GPIO1     | 25       |
| CF1              | GPIO15     | 29       |
| SEL              | GPIO2     | 26       |

Now the last step should be to plug the mains power into the PDU, flash the ESP with ESPHome and the config, calibrate the measurement as per ESPHome docs and it should be working!
Don't forget to resolder or otherwise reconnect the earth wire for safety should you have cut it (that's what i did).

Stuff the power supply in somehow and close it up, you are done!
![PXL_20250523_161857149](https://github.com/user-attachments/assets/76e7210d-e533-45a0-b6f8-cc6758daa4fc)

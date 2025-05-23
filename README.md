# LogiLink-PDU8P01-ESPHome

This repo includes:
- yaml config for the esp, including making use of the power measurement IC
- (Mostly complete) pinout for the header J1 going to the old microcontroller board
- Instructions to rebuild the PDU to use the ESP instead

&nbsp;

&nbsp;

WARNING: Due to the power distribution unit being mains powered, there is a real risk of electrocution, starting a fire or both
I do not take any responsibility should you, someone you love (or even someone you hate), get harmed or killed.

&nbsp;

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

Replace the capacitor "C5" with a connector that fits in there, i used a "PTR Hartmann STL 1550" socket and "PTR AK 1550" plug. This will be used by an external 5V 1A USB-brick that you can buy for cheap to feed the 5V for the relais and ESP. (Note that GND is on the left of the green connector, +5V on the right!)
![PXL_20250523_162205871](https://github.com/user-attachments/assets/8cd2f4d9-82ca-4861-b06a-df60ad64fd64)

I also replaced the capacitors "C4", "C18" and "C20" for piece of mind.

The USB power supply got hard-wired in, connecting the mains leads to after the fuse on one end, the other onto the main power plug of the PDU by soldering. Just be careful so that you can still plug in the wires/connectors after!
![PXL_20250523_165315783](https://github.com/user-attachments/assets/c4e8a836-e959-495a-bdb3-458c0cec52ff)
![PXL_20250523_165332505](https://github.com/user-attachments/assets/b7c7c97c-f7d9-45a4-87b5-09d9b0d76798)


This can be done in a better way for sure - i just decided to go the quick & cheap route.

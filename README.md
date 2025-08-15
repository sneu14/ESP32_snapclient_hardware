# ESP32_snapclient_hardware

This repo contains Information about connecting I2S DACs to ESO32 Boards an run a snapclient https://github.com/CarlosDerSeher/snapclient on the Board.

## DACs
### PCM5102A
The PCM5102A is available on small PCBs with an 3,5mm jack for connecting an Amplifier.
For the connection to the ESP32 the pins BCK, DIN and LCK are necessary. XSMT/3 can optionally be used if Hardware Mute/Unmute is used.
You can find some Solderpad on the PCB. Some of the Solderpads should be shortend for a stabel operation.
- The Pad between SCK and BCK on the front side of the PCB
- H-3 L (only if Hardware Mute is not used)
- H 2-L
- H 1-L
- H-4 L


## ESP32 Boards
### Olimex ESP32-PoE

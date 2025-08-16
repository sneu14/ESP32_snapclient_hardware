# ESP32_snapclient_hardware

This repo contains Information about connecting I2S DACs to ESP32 Boards and run snapclient https://github.com/CarlosDerSeher/snapclient on the Board.

## DACs
### PCM5102A
The PCM5102A is available on small PCBs (brnaded as GY PCM5102A) with a 3,5mm jack for connecting an Amplifier.
For the connection to the ESP32 the pins BCK, DIN and LCK are necessary. XSMT/3 can optionally be used if Hardware Mute/Unmute is used.
You can find some Solderpads on the PCB which should not be left floating for stable operation. This can also be realized by using a pin header and jumpers on the coresponding row of solderpads on the board.

- Short the Pad between SCK and BCK on the front side of the PCB to pull SCK low
- H-3 L (leave open if Hardware Mute is used, or pull up, i.e. connect middle pad to H to enable audio output permanently)
- H 2-L (pull down, i.e. connect middle pad to L)
- H 1-L (pull down, i.e. connect middle pad to L)
- H-4 L (pull up, i.e. connect middle pad to H)


## ESP32 Boards
### Olimex ESP32-PoE
When using the Ethernetport of the Board only a limited set of the IO Pins are available. The following configuration works with Hardware Mute
|ESP32-PoE|PCM5102A|Function|
|---------|--------|--------|
|13|SCK|Clock|
|14|LCK|Channel Select|
|32|DIN|Data|
|33|3/XSMT|Hardware Mute|


### WirelessTag WT32-ETH01
The externally accessible GPIO pins on this board have several pecularities. See the very useful repo [Unofficial guide to the WT32-ETH01](https://github.com/egnor/wt32-eth01) for reference. The following GPIO selection has proven to be working fine:

#### DAC Setup

|WT32-ETH01|PCM5102A|Function|
|---------|--------|--------|
|32|SCK|Clock|
|5|LCK|Channel Select|
|33|DIN|Data|
|15|3/XSMT|Hardware Mute|

This corresponds to the following settings in the snapclients esp-idf menuconfig tool:

* Audio Board -> Audio Board -> Custom audio board
* Audio Board -> Custom Audio Board -> DAC Chip -> TI PCM5102A based DAC
* Audio Board -> Custom Audio Board -> DAC I2C control interface:
    |Value|Description|
    |---------|--------|
    |-1|SDA Pin|
    |-1|SCK Pin|
    |0x20|I2C address|
* Audio Board -> Custom Audio Board -> I2S Master Interface:
    |Value|Description|
    |---------|--------|
    |-1|Master i2s mclk|
    |32|Master i2s bck|
    |5|Master lrck|
    |33|Master i2s data out|
* Audio Board -> Custom Audio Board -> TI PCM5102A interface Configuration:
    |Value|Description|
    |---------|--------|
    |15|Master mute/unmute for PCM5102A|
* Audio Board -> Custom Audio Board -> Logic-Level-Settings -> leave all unchecked

#### Ethernet Setup

* Snapclient Ethernet Configuration
    |Value|Description|
    |---------|--------|
    |checked|Internal EMAC|
    |LAN87xx|Ethernet PHY Device|
    |23|SMI MDC GPIO number|
    |18|SMI MDIO GPIO number|
    |16|PHY Reset GPIO number|
    |1|PHY Address|
    |unchecked|SPI Ethernet|


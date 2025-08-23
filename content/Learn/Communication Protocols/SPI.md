SPI stands for Serial Peripheral Interface, and it's considered a 4-wire interface; this is misleading, though. In reality, it's 3 wires, plus one per peripheral. It's a hierarchical protocol, using a controller/peripheral regime (master/slave). The necessary wires are:

**CIPO (MISO)** - Controller in, peripheral out. This line carries messages sent from the controller, to a peripheral 
**COPI (MOSI)** - Controller out, peripheral in. This line carries messages sent from a peripheral, to the controller
**SCK** - Serial clock. This line carries a clock signal.
**nCS** - Chip select (active low). This line determines which peripheral should read a message being sent on the CIPO line. 

A typical SPI message, from the controller to a peripheral, might look like this:
![[Pasted image 20250325173716.png|center]]

Each peripheral may have different settings for SPI, and should be consulted accordingly. That said, most SPI messages take this two-word structure (a word can be any number of bytes, in this case one):

| **Address Byte** | b7         | b6            | b5            | b4            | b3            | b2            | b1            | b0            |
| ---------------- | ---------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
|                  | read/write | address bit 6 | address bit 5 | address bit 4 | address bit 3 | address bit 2 | address bit 1 | address bit 0 |
| **Data Byte**    | **b7**     | **b6**        | **b5**        | **b4**        | **b3**        | **b2**        | **b1**        | **b0**        |
|                  | data bit 7 | data bit 6    | data bit 5    | data bit 4    | data bit 3    | data bit 2    | data bit 1    | data bit 0    |

There are a number of different configurations of SPI, which will be determined in the datasheet. It's crucial to have your SPI driver configured the same as the device you're trying to send to, otherwise unpredictable behavior will occur. Often, this means reconfiguring the driver depending on which device you're sending a message to. 

SPI devices are connected as follows. Pullup resistors are needed *only on the chip select lines*.
![[SPI wiring|center]]

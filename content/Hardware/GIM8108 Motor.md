# CAN

The motors return state information whenever a command with their address is received.

## Low Level Programming

Windows
1. Connect power and UART to USB cable to the motor and windows PC. 5V may be used for basic programming, but 24V will be needed for calibration.
2. Launch Putty. Use the following configurations. Check device manager for COM number connected to the USB to UART converter. 
	1. See driver error fix for "NOT A PROLIFIC DEVICE"

| Commincation | Serial  |
| ------------ | ------- |
| Baud         | 9216100 |
| Port         | COM#    |
3. Connect to motor. Hit s for settings. You may have to retry step 2 and 3 a few times. 

## USB to UART Driver fix
Prolific pushed an update that bricks knock off chips. Downgrading fixes the issue, but honestly its probably worth it to order a new one. 

1. Remove existing driver. 
2. Install driver 3.8.25.
## Overview
If the [[NVIDIA AGX Jetson Orin Dev|Jetson]] is [[Switch]]'s brain, the power distribution board (PDB) is it's nervous system and heart. The PDB serves as a central wiring hub, to which all electronic devices in the system can plug into for power and communication. Its primary purpose is to soft-start the motors, preventing inrush current from turning them into expensive paperweights. All firmware is written in C++ to run on a pair of STM32F302 MCUs (one for each CAN line).

## Soft-Start
The purpose of the soft start process is to limit the inrush current that occurs when an inductive load (read: motor) is connected to the battery. This huge current spike will easily burn out the motor, so an impedance must be placed in the line to limit the current. However, in normal operation, we don't want this impedance to limit our $\frac{\delta i}{\delta t}$, so we connect a low-impedance line once the voltage across the inductive load has reached the system's operating voltage. This is done using a gate driver, a few MOSFETS, and some resistors. There are two motors connected to each gate driver, so for the [[Switch]] platform there is one gate driver managing the soft start for each leg.

The PDB's soft start sequence begins when the battery is plugged in. When this happens, the gate driver controlling the soft start circuitry initializes to a state where the motors are completely disconnected. When the [[NVIDIA AGX Jetson Orin Dev|Jetson]] sends the soft start command, the gate driver opens the first set of MOSFETS, connecting the motors to the battery with a 200 $\Omega$ resistor inline. As the voltage across the motor climbs, it's measured my the STM32's ADCs, and once it reaches 20V (not quite the operating voltage, but experimentally deemed sufficient to limit the total current spike), the second set of MOSFETS is opened, connecting the motors to the battery without a resistor inline. Now, soft start is complete, and the PDB sends the [[NVIDIA AGX Jetson Orin Dev|Jetson]] a message to confirm this. As a finite state machine, the whole process looks like this:

![[softstart fsm]]

# Other duties
- Sends the current battery voltage to the [[NVIDIA AGX Jetson Orin Dev|Jetson]] at 5 Hz

## Unimplemented functionality
A number of features are designed into the PCB that have yet to be implemented in firmware. These are:
- Monitor gate drivers for failures
- Monitor CAN line status
- Set MOTOR CAN receiving ID 

## Communication Table
*Incoming:*

| Message | Meaning                | Comments                |
| ------- | ---------------------- | ----------------------- |
| 0b001   | Error, shut off motors | RESERVED, UNIMPLEMENTED |
| 0b010   | Set motor IDs          | PARTIALLY IMPLEMENTED   |
| 0b011   | Do soft start          |                         |

*Outgoing:*
The PDB sends three-byte messages with a header in the MSB and data in the remaining two. These headers instruct the recipient of the type of message, so they know how to interpret the rest of the data frame.

| Header | Remaining Contents                                   | Meaning                                   | Comments |
| ------ | ---------------------------------------------------- | ----------------------------------------- | -------- |
| 0b000  | Meaningless                                          | Waiting for command (READY)               |          |
| 0b001  | Battery voltage (MSB integer part, LSB decimal part) | This is a battery level message (BATTERY) |          |
| 0b010  | Meaningless                                          | Requested action complete (CONFIRM)       |          |
| 0b011  | Meaningless                                          | Error (ERROR)                             |          |

## Files

| Filename                      | Purpose                                                                                                     | Status                         |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------ |
| main.cpp                      | Main file for firmware                                                                                      | CORE FUNCTIONALITY IMPLEMENTED |
| gate_driver.h/gate_driver.cpp | Object definitions for gate driver                                                                          | PARTIALLY IMPLEMENTED          |
| LED_driver.h/LED_driver.cpp   | Object definitions for LED driver                                                                           | COMPLETE                       |
| motors.h/motors.cpp           | Object definitions for motor ID class                                                                       | UNIMPLEMENTED                  |
| Quadruped PDB - MCU 2.ioc     | Pin configuration file for the chip. Currently, both MCU pinouts are identical, so this is flashed to both. | COMPLETE                       |

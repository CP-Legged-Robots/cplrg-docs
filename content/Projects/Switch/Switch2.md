Author: Kai De La Cruz

# Overview

Switch-2 is a custom 12 degree of freedom quadruped continuing the [[Switch]] 
platform by incorporating hip roll motors in each leg. 

![[Switch2.jpeg]]

Switch-2 continues the modular design philosophy of its predecessor by providing
an adaptable hardware testbed for future works. The mechanical design, 
manufacturing, and initial validation of this system was performed from 
2024-2026 for the thesis work of [[Kai De La Cruz]]. 

#### Objectives 
1. Redesign structural hardware for 12-DOF system.
2. Produce more dynamically capable robot.
3. Maintain modularity for future research.

# Hardware

Switch-2 reuses much of the same components featured in the original 
Switch [Hardware](./Switch.md#software). The following components are either 
already or are intended to be implemented in the future. 

[[GIM8108 Motor]]
[[BNO08x]]
[[NVIDIA AGX Jetson Orin Dev]]

The notable change would be the switch to [[Li-ion Batteries]]. Li-ion batteries
were purchased and mounted to the chassis but are yet to be implemented with the 
PDB system. 

# Wired Communication

The wired communication for Switch-2 carries over the same set up as the 
original Switch with the addition of hip roll motors. This results in 2 more 
motors on a given CAN bus. 

![[Wiring Diagram.png]]

**NOTE:** For single-leg testing, only CAN bus 0 was used. 

# Software

The same development stack used in the original Switch [Software](./Switch.md#software) section was reused. However, the default ROS middleware was used
instead of [[Zenoh]]. 

To connect to Switch-2, the same wireless communication protocol was used as described in [Wireless Communication](./Switch.md#software). 

# Tutorials

## Single-leg Gazebo Simulation

## Single-leg Hardware Testing

### Remote Access the Jetson

1. Power on the Jetson while close to the linked router.
2. Connect remote access computer to router. 
3. Open [[NoMachine]] on remote access computer.
4. Click on "orin" to connect to the Jetson.
5. Setup system monitor and enter password if prompted.

**Additional Resources**
[Jetson Hacks NoMachine Tutorial](
https://www.youtube.com/watch?v=OYrSADrtSag)

### Startup Switch-2 Coding Environment

**Prerequisites**
Remote access the Jetson.

1. Connect to Jetson through NoMachine.
2. Open Switch-2 hardware project in VSCode.
    * Ensure this repo is cloned to Jetson: [SL hardware project](
      https://github.com/samurai-kai/quadruped_hardware)
3. Open reopen project in docker container.
    * Ensure the Docker and devcontainer extensions are installed for VSCode. 
    * Press "Open a remote window" button in bottom left corner of VSCode, then
    "Reopen in Container" from menu.
4. The system should be operational and ready to start

**Additional Resources**

[Devcontainer startup at 2:40](
https://www.youtube.com/watch?v=X7guekGZM20)

### Startup Switch-2 ROS2 Environment

**Prerequisites**
Startup Switch-2 coding environment (dev/Docker container) .

1. To use ROS2, source  Jazzy in one terminal by running `source startup.sh`
2. In all additional terminals run `source new.sh`

### Startup Hardware
1. Complete [[Switch Pre-Startup Checklist]].
2. Startup CAN bus with `source enable_CAN.sh`
    * Run this in a terminal outside of VSCode.
    * The file location should be in the Jetson Documents
3. Connect the battery to Switch. 
    * CHECK AND UNDERSTAND VOLTAGE LIMITS BEFORE USE. YOU CAN BLOW UP A BATTERY.
	* [[LiPo Batteries]]
	* Messages on both busses should be reporting battery voltage in hex. 
4. Complete [[Switch Operation Checklist]] to ensure everything is working smoothly.
5. Launch the real hardware controller manager to startup hardware. `source rebuild.sh`
    * Indicator lights on motors should all turn green after running this step.
    * A ______ confirmation message in terminal will appear to indicate a 
    success.

### Running Zeroing

**Prerequisites**
Started hardware

1. Move robot into defined zero position and maintain zero position by hand. 
2. In a new terminal, launch zeroing controller. `source zerocontrol.sh`  
    * Switch-2 will immediately set zero states upon running this step.
4. Inspect terminal for errors.
5. Close zeroing controller to free up command interfaces. 
`source CLdeactivatezero.sh`
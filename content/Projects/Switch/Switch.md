# Switch
Author: Jeremy West

# Overview

The Cal Poly Switch is a custom 8 degree of freedom quadrupedal robot and educational development platform to give students absolute freedom for legged robotics development. 

![[Switch_standing.jpg]]

The Cal Poly Legged Robotics Switch project is an advanced quadrupedal robot application that is modular and mobile written in ROS2. The software and hardware were designed to be encapsulated so future work can pull built solutions and not rework finished solutions. The Switch project was performed from 2023-2025 for Jeremy West's thesis. 

#### Objectives
1. Improve mechanical design for integration with new hardware components.
2. Design a wireless mobile robotics development stack to achieve increased mobility.
3. Maintain modularity in hardware and software drivers for long term utility. 

A software stack was created to solve several common issues in mobile robotics. 
####  Stack Objectives
1. Parallel wireless development.
2. Real time task management for robotic operations.
3. Robust environment handling to handle scaling complexity.

# Hardware
The Switch uses FOC brushless motors, a 9 axis IMU, a lithium polymer battery with a custom power distribution board, and an on board NVIDIA AGX Jetson Orin Dev. An overview of the hardware may be observed below. 

[[GIM8108 Motor]]
[[BNO08x]]
[[LiPo Batteries]]
[[NVIDIA AGX Jetson Orin Dev]]

# Wired Communication
![[comms.drawio.png]]

# Software

[[NoMachine]]
[[Foxglove]]
[[ROS2]]
[[ROS2 Control]]
[[VSCode]]
[[DevContainer]]
[[Docker]]
[[CAN Utils]]

# Wireless Communication

![[SwitchWirelessComms.png]]







# Tutorials

## Remote Access the Jetson

1. Power on the Jetson while close to the linked router.
2. Connect remote access computer to router. 
3. Open [[NoMachine]] on remote access computer.
4. Click on "orin" to connect to the Jetson.
5. Setup system monitor and enter password if prompted.

**Additional Resources**
Jetson Hacks NoMachine Tutorial
https://www.youtube.com/watch?v=OYrSADrtSag

## Startup Switch Coding Environment

**Prerequisites**
Remote access the Jetson.

1. Connect to Jetson through NoMachine.
2. Open Switch project in VSCode.
3. Open reopen project in docker container.
	1.  Ensure the Docker and devcontainer extensions are installed for VSCode. 
4. The system should be operational and ready to start

## Startup Switch ROS2 Environment

**Prerequisites**
Startup Switch coding environment (dev/Docker container) .

1. To use Zenoh start up the router in one terminal.
	`source CLrouterstart.sh`
2. In all additional terminals run 
		`source CLNew.sh`

**Additional Resources**

Devcontainer startup at 2:40
https://www.youtube.com/watch?v=X7guekGZM20
## Startup Hardware
1. Complete [[Switch Pre-Startup Checklist]].
2. Startup CAN bus with 
	`source enable_CAN.sh`
3. Connect the battery to Switch. CHECK AND UNDERSTAND VOLTAGE LIMITS BEFORE USE. YOU CAN BLOW UP A BATTERY.
	1. [[LiPo Batteries]]
	2. Messages on both busses should be reporting battery voltage in hex. 
4. Start IMU Node.
	1. `ros2 run bno08x_driver bno08x_driver`
5. Launch the real hardware controller manager to startup hardware.
		`ros2 launch quadruped_bringup real.launch.py`
6. Complete [[Switch Operation Checklist]] to ensure everything is working smoothly.

## Running Zeroing

**Prerequisites**
Started hardware

1. Move robot into pre-zeroing position on a flat surface. 
2. Launch zeroing controller 
	1. `ros2 launch quadruped_utils testcontroller.launch.py` 
3. Wait for Switch to finish zeroing.
4. Inspect terminal for errors.
5. Close zeroing controller to free up command interfaces with
	1. `source CLstopzero.sh`
## Starting Foxglove

**Prerequisites**
Running zeroed hardware for data or a ROS bag file.

1. Launch Foxglove.
	1. `source CLFoxglove `
2. Open Foxglove in your browser and access the port or open the VS code popup.

# Debugging



![[SystemDependencies.drawio.png]]

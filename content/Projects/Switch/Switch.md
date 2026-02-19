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

The Switch runs on board communication connected to the NVIDIA Jetson AGX
Orin through 2 standard protocols: I2C and CAN. The I2C bus is connected to the
BNO08x IMU for body orientation measurements. The CAN communication is split
to 2 buses to reduce bandwidth. A diagram of the communication system of the
Switch is denoted below.

![[comms.drawio.png]]

The motors on board the Switch are controlled over the CAN0 and CAN1 busses.
These buses are also connected the Power Distribution Board (PDB) to access battery
and power state information. Note while CAN bus 0 and CAN bus 1 are both
connected to the PDB, they communicate with the 2 separate ICs. The 2 CAN
buses are not connected together and are independent. The Jetson has ports for
CAN in and out, which pass into a CAN transceiver to convert to standard bus CAN
high and CAN low.
# Software

[[NoMachine]]
[[Foxglove]]
[[ROS2]]
[[ROS2 Control]]
[[VSCode]]
[[DevContainer]]
[[Docker]]
[[CAN Utils]]

The software in the tech stack may be organized into roles of remote access, robotic
framework, and environment handling. An overview of the development stack is
shown below.

![[stack.png]]

Starting at the lower level, [[VSCode]] was used as a text editor in conjunction with the
development container extension and a project specific docker container to create a
full coding environment. To handle dependencies, the [[Docker]] file creates a stripped
down virtual machine for the user to run their code in. The [[DevContainer]] (developer container) turns this docker file into a full coding environment in VSCode, with Linux terminal access, and intuitive editing on project files. Additionally the developer container
file handles all permission configurations and port routing simplifying the running
process of the Docker file. The end result is an encapsulated coding environment to
handle all background configuration for the project.

[[ROS2]] and [[ROS2 Control]] handle the robot framework with [[Zenoh]] a Rust based Web-
Sockets manager handling data transfer. Drivers and controllers are implemented in
ROS2 Control and programs are managed through ROS2. ROS2 jazzy, Zenoh, and
their dependencies are listed in the Docker file. Configurations and port forwarding
necessary for the wireless communications used by ROS are set up in the dev con-
tainer. For development on board the mobile robot, [[NoMachine]] was used for remote
admin access of the Jetson over WiFi. This allows full capabilities for development of
the mobile robot without tangles or range limits introduced by wired communication.

The local network also manages ROS2 sessions. Any machine connected to the router
can access the active ROS2 session on board the Jetson with use of the ROS2 middle
ware. A user running ROS may then open up a foxglove web socket to run the
[[Foxglove]] interface locally for data visualization and debugging.



# Wireless Communication

Wireless communication on the Switch is hosted by a router. By connecting the Jetson and the user, different services may be utilized. 

![[SwitchWirelessComms.png]]

Standard operation sees a user remote into the Jetson via NoMachine for full Admin access to the machine. This remote access user has all of the same capabilities of a user who hooked up a display and keyboard directly to the Jetson. 

Additionally through the use of ROS2 middleware, additional ROS2 users can gain access to the ROS2 workspace, meaning they can monitor data and even run remote control with a lower level of access. 

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

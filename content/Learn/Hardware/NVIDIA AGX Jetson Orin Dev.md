# Overview
The Jetson series are ARM based SOC computers designed for mobile robotics and machine learning. You can think of them as a very powerful Raspberry Pi. 3 main things set them apart from other computers.

1. High AI performance while being both...
2. Low Power.
3. Low mass.

The NVIDIA AGX Jetson Orin Dev in 2024 was NVIDIA's highest performance Jetson Device with 275 TFLOPS (Trillion Floating Point Operations Per Second). Much like a GPU the Jetson's hardware is specialized for massive parallel operations, however it was designed to be very power efficient. The cost is that the Jetson does not scale in performance as more power is added in the same way that traditional computers function. 

The key to taking advantage of the Hardware on board Jetson is the use of [[CUDA]] NVIDIA's proprietary hardware management software. When writing code in C and Python the user may specify to run the selected process using [[CUDA]]. [[CUDA]] will then decide the most efficient way to run the software using all available hardware, including the CPU. 
# CAN
The Jetson utilizes 2 CAN busses CAN0 and CAN1. The pins need to be formatted very specifically to operate as CAN pins every startup and sometimes after a crash. A shell file was made to simplify this process. 

## Enable CAN

1. `source enable_CAN.sh`

For low level testing it is beneficial to use the CAN line for verification. The jetson uses the [[Linux]] [[CAN Utils]] package. 

NVIDIA CAN guide
https://docs.nvidia.com/jetson/archives/r35.4.1/DeveloperGuide/text/HR/ControllerAreaNetworkCan.html




# Resources

A Forum Post I made
https://forums.developer.nvidia.com/t/socket-can-sending-incorrect-messages/322038/6

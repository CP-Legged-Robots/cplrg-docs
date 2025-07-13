Linux Download
	`sudo apt-get can-utils`


# Command Line Commands

Send on can bus 0 to ID 123 message "abcabc"
	cansend can0 123#abcabc

dump all received CAN messages
	candump -x any & 

dump all received CAN messages with TX RX and timestamp
candump -ta -x -c -c can0 can1

dump all received CAN messages with TX RX and timestamp, filter out "002"
candump -ta -x -c -c can0 can1 | grep -v  "002"

Check bus status
ip link show dev can0
ip link show dev can1


**Resources**

NVIDIA CAN setup and CAN Utils guide
https://docs.nvidia.com/jetson/archives/r35.4.1/DeveloperGuide/text/HR/ControllerAreaNetworkCan.html

A 
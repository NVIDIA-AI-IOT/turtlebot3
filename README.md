# turtlebot3

# Hardware Setup
The Turtlebot3 hardware is all open source, and you can find the STL files for the base plates of the burger and waffle on their wiki. 
 ![](https://github.com/NVIDIA-Jetson/turtlebot3/blob/master/3dprint.png)

# Setting up your Jetson as SBC
 We are using the Jetson TX2 as the SBC (Single Board Computer) to control the turtlebot3 waffle. The Dynamixel motors on the Turtlebot3 are controlled by the OpenCR board. The OpenCR board connection is an ACM port to the Jetson, which is disabled in the kernel, and so is the cp210 port to the HLS-LFCD LDS lidar that comes with the Turtlebot. In order to reconfigure the kernel, first flash your Jetson with Jetpack 3.0. Follow the instructions [here](http://www.jetsonhacks.com/2017/03/21/jetpack-3-0-nvidia-jetson-tx2-development-kit/).

 
 Next, follow the Jetsonhacks instructions [here](http://www.jetsonhacks.com/2017/03/25/build-kernel-and-modules-nvidia-jetson-tx2/) to reconfigure the kernel and stop after running
 `./getKernelSources.sh`
 The last line
 `make xconfig`
 will bring up the GUI for kernel configuration. Enable the ACM and cp210 ports:
 ![](https://github.com/NVIDIA-Jetson/turtlebot3/blob/master/kernelcp210.png)
 ![](https://github.com/NVIDIA-Jetson/turtlebot3/blob/master/kernelacm.png)
 
 Make sure to save your changes to the config file, and go through the rest of the instructions to finish making the kernel. If no errors occur, reboot the Jetson. Plug in your OpenCR board and Lidar, and `cd /dev` in terminal. If the kernel reconfiguration worked, you should be able to see 'ttyACM0' and 'ttyUSB0'.

# Installing ROS 
Follow the instructions 
[here](http://www.jetsonhacks.com/2017/03/27/robot-operating-system-ros-nvidia-jetson-tx2/). 
to install ROS kinetic on your TX2 and set up your catkin workspace.

Follow the instructions on the Turtlebot3 wiki for the waffle from 6.3.1 and on, to install the Turtlebot3 dependencies and clone the repositories. 
[here](http://turtlebot3.readthedocs.io/en/latest/sbc_software.html) 

When configuring the network, make sure that you specify the IP addresses of our ROS_MASTER and your ROS_HOST correctly. On your Jetson, you want to make the ROS_MASTER_URI the IP of your remote host, and the ROS_HOSTNAME the IP of your Jetson. After modifying the bashrc file, make sure to 
 `source ~/.bashrc`

 Follow the instructions [here](http://turtlebot3.readthedocs.io/en/latest/pc_software.html) to set up your remote host, which is a PC that you will be using to run roscore and send commands to the Turtlebot.

  # ROS Navigation Stack Tuning Tips

After setting up the ROS navigation stack and discovering that the navigation works, but not well, how do you tune it?

Setting up the Turtlebot3 took only one day, and generating a decent map took another few days, but tuning navigation and adding another level of autonomy by leaving Rviz was a 2 week adventure. 

https://arxiv.org/pdf/1706.09068.pdf 

<p align="center">
  <img src="https://github.com/NVIDIA-Jetson/turtlebot3/blob/master/images/electron.png">
</p>

### Hardware Setup
The Turtlebot3 hardware is all open source, and you can find the STL files for the base plates of the burger and waffle on their    wiki. You can 3D print extra base plates and modify your Turtlebot3 however you would like. 

<p align="center">
  <img src="https://github.com/NVIDIA-Jetson/turtlebot3/blob/master/images/3dprint.png" width="500">
</p>

Below is an example of a 3 level mod of the TB3 Burger. It comes with 8 base plates, and I 3D printed another one to create a 3 level mod with 3 plates on each level. This mod is more stable than the 4 level robot, and also allows for more room to add additional sensors.

<p align="center">
  <img src="https://github.com/NVIDIA-Jetson/turtlebot3/blob/master/images/3levelmodburger.png" width="500">
</p>

I put the entire development board with the Jetson on the 2nd level of the waffle, but on the burger there is only room for a carrier board. I used an [Auvidea J120](https://auvidea.com/j120/) and I removed the heat sink from the Jetson because it was too tall.

<p align="center">
  <img src="https://github.com/NVIDIA-Jetson/turtlebot3/blob/master/images/carrierboard.png" width="500">
</p>

The Turtlebot3 comes with a [HLS-LFCD2](http://turtlebot3.readthedocs.io/en/latest/appendix_lds.html), which is a 360 degree LIDAR, but you can also add other sensors like the [ZED](https://www.stereolabs.com/) or [Tara](https://www.e-consystems.com/3D-USB-stereo-camera.asp) stereocam to it. 

<p align="center">
  <img src="https://github.com/NVIDIA-Jetson/turtlebot3/blob/master/images/wafflezed.png" width="500">
</p>


# Setting up your Jetson as SBC
 We are using the Jetson TX2 as the SBC (Single Board Computer) to control the turtlebot3 waffle. The Dynamixel motors on the Turtlebot3 are controlled by the OpenCR board. The OpenCR board connection is an ACM port to the Jetson, which is disabled in the kernel, and so is the cp210x port to the HLS-LFCD LDS lidar that comes with the Turtlebot. In order to reconfigure the kernel, first flash your Jetson with Jetpack 3.0. Follow the instructions [here](http://www.jetsonhacks.com/2017/03/21/jetpack-3-0-nvidia-jetson-tx2-development-kit/).
 
 Next, follow the Jetsonhacks instructions [here](http://www.jetsonhacks.com/2017/03/25/build-kernel-and-modules-nvidia-jetson-tx2/) to reconfigure the kernel and stop after running
 `./getKernelSources.sh`
 The last line
 `make xconfig`
 will bring up the GUI for kernel configuration. Enable the ACM and cp210x ports. The ACM port is for the OpenCR board, and the cp210x is for the LIDAR (either the HLS-LFCD LIDAR or the RPLidar A1/A2)
 
 <p align="center">
  <img src="https://github.com/NVIDIA-Jetson/turtlebot3/blob/master/images/kernelcp210.png" width="500">
</p>
<p align="center">
  <img src="https://github.com/NVIDIA-Jetson/turtlebot3/blob/master/images/kernelacm.png" width="500">
</p>
 
 Make sure to save your changes to the config file, and go through the rest of the instructions to finish making the kernel. If no errors occur, reboot the Jetson. Plug in your OpenCR board and Lidar, and `cd /dev` in terminal. If the kernel reconfiguration worked, you should be able to see 'ttyACM0' and 'ttyUSB0'.

# Installing ROS 
Follow the instructions 
[here](http://www.jetsonhacks.com/2017/03/27/robot-operating-system-ros-nvidia-jetson-tx2/). 
to install ROS kinetic on your TX2 and set up your catkin workspace.

Follow the instructions on the Turtlebot3 wiki for the waffle from 6.3 and on, to install the Turtlebot3 dependencies and clone the repositories. 
[here](http://emanual.robotis.com/docs/en/platform/turtlebot3/pc_setup/#install-dependent-packages)

When configuring the network, make sure that you specify the IP addresses of our ROS_MASTER and your ROS_HOST correctly. On your Jetson, you want to make the ROS_MASTER_URI the IP of your remote host, and the ROS_HOSTNAME the IP of your Jetson. After modifying the bashrc file, make sure to 
 `source ~/.bashrc`

 Follow the instructions [here](http://turtlebot3.readthedocs.io/en/latest/pc_software.html) to set up your remote host, which is a PC that you will be using to run roscore and send commands to the Turtlebot.

  # ROS Navigation Stack Tuning Tips

After setting up the ROS navigation stack and discovering that the navigation works, but not well, how do you tune it?

https://arxiv.org/pdf/1706.09068.pdf 

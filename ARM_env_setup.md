# install Jetpack 4.6
```
https://docs.nvidia.com/jetson/jetpack/install-jetpack/index.html
```

# Install ROS(melodic)
## Installation
Configure your Ubuntu repositories
Configure your Ubuntu repositories to allow "restricted," "universe," and "multiverse." You can follow the Ubuntu guide for instructions on doing this.

Setup your sources.list
Setup your computer to accept software from packages.ros.org.

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

Mirrors

Source Debs are also available

Set up your keys
```
sudo apt install curl # if you haven't already installed curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```
Installation
First, make sure your Debian package index is up-to-date:

sudo apt update
There are many different libraries and tools in ROS. We provided four default configurations to get you started. You can also install ROS packages individually.

In case of problems with the next step, you can use following repositories instead of the ones mentioned above ros-shadow-fixed
Desktop-Full Install: (Recommended) : ROS, rqt, rviz, robot-generic libraries, 2D/3D simulators and 2D/3D perception
```
sudo apt install ros-melodic-desktop-full
```

Desktop Install: ROS, rqt, rviz, and robot-generic libraries
```
sudo apt install ros-melodic-desktop
```


ROS-Base: (Bare Bones) ROS package, build, and communication libraries. No GUI tools.
```
sudo apt install ros-melodic-ros-base
```


Individual Package: You can also install a specific ROS package (replace underscores with dashes of the package name):

```
sudo apt install ros-melodic-PACKAGE
```

e.g.
sudo apt install ros-melodic-slam-gmapping
To find available packages, use:

apt search ros-melodic

Environment setup
It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell is launched:


```
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
If you have more than one ROS distribution installed, ~/.bashrc must only source the setup.bash for the version you are currently using.

If you just want to change the environment of your current shell, instead of the above you can type:


```source /opt/ros/melodic/setup.bash```
If you use zsh instead of bash you need to run the following commands to set up your shell:


```
echo "source /opt/ros/melodic/setup.zsh" >> ~/.zshrc
source ~/.zshrc
```

Dependencies for building packages
Up to now you have installed what you need to run the core ROS packages. To create and manage your own ROS workspaces, there are various tools and requirements that are distributed separately. For example, rosinstall is a frequently used command-line tool that enables you to easily download many source trees for ROS packages with one command.

To install this tool and other dependencies for building ROS packages, run:


```
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
```
Initialize rosdep
Before you can use many ROS tools, you will need to initialize rosdep. rosdep enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS. If you have not yet installed rosdep, do so as follows.


```
sudo apt install python-rosdep
```
With the following, you can initialize rosdep.


```
sudo rosdep init
rosdep update
```

# Install realsensesdk(version should be 2.45.0)
## Building from Source using RSUSB Backend
Use the RSUSB backend without the kernel patching

### Download librealsense sdk v2.45.0 source code
```
cd
git clone https://github.com/IntelRealSense/realsense-ros.git
cd librealsense
git checkout tags/v2.45.0
```

### Install librealsense with CUDA
In order to build the SDK using the RSUSB method and avoid the kernel patching procedure, please refer to libuvc_installation.sh script for details. If you have CUDA dev-kit installed, don't forget to add -DBUILD_WITH_CUDA=true for optimal performance.
```
cd script
./libuvc_installation.sh -DBUILD_WITH_CUDA=true
```

# Install realsenseros( version should be match to realsensesdk v.2.45.0)

# install dependency

```
sudo apt-get install ros-melodic-ddynamic-reconfigure
```

# ROS Wrapper for Intel&reg; RealSense&trade; Devices

## Installation Instructions

   *Ubuntu*
   ```bash
   mkdir -p ~/catkin_ws/src
   cd ~/catkin_ws/src/
   ```

   - Clone the latest Intel&reg; RealSense&trade; ROS from [here](https://github.com/intel-ros/realsense/releases) into 'catkin_ws/src/'
   ```bashrc
   git clone https://github.com/IntelRealSense/realsense-ros.git
   cd realsense-ros/
   git checkout `git tag | sort -V | grep -P "^2.\d+\.\d+" | tail -1`
   cd ..
   ```
   - Make sure all dependent packages are installed. You can check .travis.yml file for reference.
   - Specifically, make sure that the ros package *ddynamic_reconfigure* is installed. If *ddynamic_reconfigure* cannot be installed using APT or if you are using *Windows* you may clone it into your workspace 'catkin_ws/src/' from [here](https://github.com/pal-robotics/ddynamic_reconfigure/tree/kinetic-devel)

   - I don't know why this is needed.....
   ```bash
   cd /usr/include
   sudo cp -a opencv4 opencv
   ```

   ```bash
  catkin_init_workspace
  cd ..
  catkin_make clean
  catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release
  catkin_make install
  ```

  *Ubuntu*
  ```bash
  echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
  source ~/.bashrc
  ```


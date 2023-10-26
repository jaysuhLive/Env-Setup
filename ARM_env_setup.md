# install ubuntu on nuc (should install with usb key/mouse)
- 1. Format ssd
- 2. set boot priority USB image to top
- 3. install ubuntu with grub nomodeset command
- 4. set disk partion as swap area - (512mb), / (root, all), EFI - (512mb)
- 5. bootloader should be default

# boot with esc to pop grub
- press e to edit grub command line
- add nomodeset right after 'quite splash'

# successfully boot, set mod grub command line
```
sudo gedit /etc/default/grub
```
- add 'nomodeset' to 'GRUB_CMD_LINE_LINUX_DEFAULT="quite splash"', it should be seen like this
```
GRUB_CMD_LINE_LINUX_DEFAULT="quite splash nomodeset"
```
- updqte grub
```
sudo update-grub2
```

# ubuntu nvidia driver crash
- if ubuntu login screen freezes and the other i/o devices not working just reinstall xorg
- get to recovery mode on grub
```
sudo apt-get purge xorg-*
sudo apt-get purge xserver-xorg (if it is available)

sudo apt-get install xorg
sudo apt-get insatll xerver-xorg (if it is available)
```
# install nvidia driver on nuc
- 1. etc/modprobe.d/blacklist.conf 에서 blacklist nouveau 추가
- 2. update
```
sudo update-initramfs -u
```
- 3. reboot
- 4. install nvidia driver with command
```
sudo apt-get install -y nvidia-driver-470 (for mine)
```
# install cuda(runfile)
- 1. download cuda dev kit -> https://developer.nvidia.com/cuda-11-4-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=18.04&target_type=runfile_local
- 2. click runfile(local) option, it'll be seen like below
- 3. be sure not check nvidia drvier to install (already installed)
```
wget https://developer.download.nvidia.com/compute/cuda/11.4.0/local_installers/cuda_11.4.0_470.42.01_linux.run
sudo sh cuda_11.4.0_470.42.01_linux.run
```
- 4. path register to zshrc or profile
```
export PATH=$PATH:/usr/local/cuda-11.4/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.4/lib64
export CUDADIR=/usr/local/cuda-11.4
export CUDA_HOME=/usr/local/cuda
export PATH=$PATH:$CUDA_HOME/bin
```
# install cuda(deb)
- 1. download & isntall
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.4.0/local_installers/cuda-repo-ubuntu1804-11-4-local_11.4.0-470.42.01-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804-11-4-local_11.4.0-470.42.01-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu1804-11-4-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda
```
- 2. path register to zshrc or profile
```
export PATH=$PATH:/usr/local/cuda-11.4/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.4/lib64
export CUDADIR=/usr/local/cuda-11.4
export CUDA_HOME=/usr/local/cuda
export PATH=$PATH:$CUDA_HOME/bin
```
# install cudnn
- 1. download cudnn -> https://developer.nvidia.com/cudnn
- 2. unzip and cp to local folder
```
tar xvzf cudnn-11.4-linux-x64-v8.0.5.39.tgz
sudo cp cuda/include/cudnn* /usr/local/cuda-11.4/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda-11.4/lib64
sudo chmod a+r /usr/local/cuda-11.4/include/cudnn.h /usr/local/cuda-11.4/lib64/libcudnn*
```
- 3. link symboic link
```
sudo ldconfig
```
```
sudo ln -sf /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_adv_train.so.8.0.5 /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_adv_train.so.8
```
```
sudo ln -sf /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8.0.5  /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8
```
```
sudo ln -sf /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8.0.5  /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8
```
```
sudo ln -sf /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8.0.5  /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8
```
```
sudo ln -sf /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_ops_train.so.8.0.5  /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_ops_train.so.8
```
```
sudo ln -sf /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8.0.5 /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8
```
```
sudo ln -sf /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn.so.8.0.5  /usr/local/cuda-11.4/targets/x86_64-linux/lib/libcudnn.so.8
```
- 4. check!
```
ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn
```

# opencv_env_setting

## Install dependency

- 1. 기존 버전이 설치되어있는지를 확인 후 설치가 안 되어있으면 설치를 진행하면 됨
```
pkg-config --modversion opencv
```
 
- 2. 제거
```
sudo apt-get purge libopencv* python-opencv
sudo apt-get autoremove
 
sudo find /usr/local/ -name "*opencv*" -exec rm -i {} \;
```

- 3. 준비

```
sudo apt-get update
sudo apt-get upgrade
 
sudo apt-get install build-essential cmake
sudo apt-get install pkg-config
sudo apt-get install libjpeg-dev libtiff5-dev libpng-dev
 
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libxvidcore-dev libx264-dev libxine2-dev
 
sudo apt-get install libv4l-dev v4l-utils
 
sudo apt-get install libstreamer1.0-dev libstreamer-plugins-base1.0-dev
 
sudo apt-get install libgtk2.0-dev
 
sudo apt-get install libgtk-3-dev
sudo apt-get install libqt4-dev
sudo apt-get install libqt5-dev
 
sudo apt-get install mesa-utils libgl1-mesa-dri libgtkgl2.0-dev libgtkglext1-dev
 
sudo apt-get install python2.7-dev python3-dev python-numpy python3-numpy
```

## Download OPENCV
```
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.4.0.zip
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.4.0.zip

unzip opencv.zip
unzip opencv_contrib.zip
```

## Cmake option
- make folder in opencv folder
```
mkdir build
```
```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D WITH_TBB=OFF \
-D WITH_IPP=OFF \
-D WITH_1394=OFF \
-D BUILD_WITH_DEBUG_INFO=OFF \
-D BUILD_DOCS=OFF \
-D BUILD_EXAMPLES=OFF \
-D BUILD_TESTS=OFF \
-D BUILD_PERF_TESTS=OFF \
-D WITH_CUDA=ON \
-D WITH_CUDNN=ON \
-D OPENCV_DNN_CUDA=ON \
-D CUDA_FAST_MATH=ON \
-D CUDA_ARCH_BIN=7.5 \
-D WITH_CUBLAS=ON \
-D WITH_CUFFT=ON \
-D WITH_QT=ON \
-D WITH_GTK=OFF \
-D WITH_OPENGL=ON \
-D WITH_V4L=ON \
-D WITH_FFMPEG=ON \
-D WITH_XINE=ON \
-D BUILD_NEW_PYTHON_SUPPORT=ON \
-D INSTALL_C_EXAMPLES=OFF \
-D INSTALL_PYTHON_EXAMPLES=OFF \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.4.0/modules \
-D OPENCV_ENABLE_NONFREE=ON \
-D BUILD_EXAMPLES=OFF ..
```

```
make -j$
sudo make install
sudo ldconfig
```
## After install
```
sudo make install
위의 명령어로 인스톨을 하고,
 
아래 명령어로 확인해서 /usr/local/lib이 있으면 된 것입니다.
cat /etc/ld.so.conf.d/*
 
해당 파일이 없다면,
sudo sh -c 'echo '/usr/local/lib' > /etc/ld.so.conf.d/opencv.conf'
을 하고,
 
모두 완료되었으면
sudo ldconfig
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
# Linux Distribution

# install specific apt version(depends on version)
```
sudo apt-get install librealsense2-udev-rules=2.45.0-0\~realsense0.7525
sudo apt-get install librealsense2=2.45.0-0\~realsense0.7525
sudo apt-get install librealsense2-dkms=1.3.18-0ubuntu1
sudo apt-get install librealsense2-gl=2.45.0-0\~realsense0.7525
sudo apt-get install librealsense2-net=2.45.0-0\~realsense0.7525
sudo apt-get install librealsense2-utils=2.45.0-0\~realsense0.7525
```

#### Using pre-build packages
**Intel® RealSense™ SDK 2.0** provides installation packages for Intel X86/AMD64-based Debian distributions in [`dpkg`](https://en.wikipedia.org/wiki/Dpkg) format for Ubuntu 16/18/20/22 [LTS](https://wiki.ubuntu.com/LTS).    
The Realsense [DKMS](https://en.wikipedia.org/wiki/Dynamic_Kernel_Module_Support) kernel drivers package (`librealsense2-dkms`) supports Ubuntu LTS kernels 4.4, 4.8, 4.10, 4.13, 4.15, 4.18*, 5.0*, 5.3*, 5.4, 5.13 and 5.15. Please refer to [Ubuntu Kernel Release Schedule](https://wiki.ubuntu.com/Kernel/Support) for further details.

#### Configuring and building from the source code
While we strongly recommend to use DKMS package whenever possible, there are certain cases where installing and patching the system manually is necessary:
 - Using SDK with non-LTS Ubuntu kernel versions: **4.16 **
 - Integration of user-specific patches/modules with `librealsense` SDK.
 - Adjusting the patches for alternative kernels/distributions.

The steps are described in [Linux manual installation guide](./installation.md)


## Installing the packages:
- Register the server's public key:  
`sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE`
In case the public key still cannot be retrieved, check and specify proxy settings: `export http_proxy="http://<proxy>:<port>"`  
, and rerun the command. See additional methods in the following [link](https://unix.stackexchange.com/questions/361213/unable-to-add-gpg-key-with-apt-key-behind-a-proxy).  

- Add the server to the list of repositories:  
`sudo add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main" -u`  

- Install the libraries (see section below if upgrading packages):  
  `sudo apt-get install librealsense2-dkms`  
  `sudo apt-get install librealsense2-utils`  
  The above two lines will deploy librealsense2 udev rules, build and activate kernel modules, runtime library and executable demos and tools.  

- Optionally install the developer and debug packages:  
  `sudo apt-get install librealsense2-dev`  
  `sudo apt-get install librealsense2-dbg`  
  With `dev` package installed, you can compile an application with **librealsense** using `g++ -std=c++11 filename.cpp -lrealsense2` or an IDE of your choice.

Reconnect the Intel RealSense depth camera and run: `realsense-viewer` to verify the installation.

Verify that the kernel is updated :    
`modinfo uvcvideo | grep "version:"` should include `realsense` string

## Upgrading the Packages:
Refresh the local packages cache by invoking:  
  `sudo apt-get update`  

Upgrade all the installed packages, including `librealsense` with:  
  `sudo apt-get upgrade`

To upgrade selected packages only a more granular approach can be applied:  
  `sudo apt-get --only-upgrade install <package1 package2 ...>`  
  E.g:   
  `sudo apt-get --only-upgrade install  librealsense2-utils librealsense2-dkms`  

## Uninstalling the Packages:
**Important** Removing Debian package is allowed only when no other installed packages directly refer to it. For example removing `librealsense2-udev-rules` requires `librealsense2` to be removed first.

Remove a single package with:   
  `sudo apt-get purge <package-name>`  

Remove all RealSense™ SDK-related packages with:   
  `dpkg -l | grep "realsense" | cut -d " " -f 3 | xargs sudo dpkg --purge`  

## Package Details:
The packages and their respective content are listed below:  

Name    |      Content   | Depends on |
-------- | ------------ | ---------------- |
librealsense2-udev-rules | Configures RealSense device permissions on kernel level  | -
librealsense2-dkms | DKMS package for Depth cameras-specific kernel extensions | librealsense2-udev-rules
librealsense2 | RealSense™ SDK runtime (.so) and configuration files | librealsense2-udev-rules
librealsense2-utils | Demos and tools available as a part of RealSense™ SDK | librealsense2
librealsense2-dev | Header files and symbolic link for developers | librealsense2
librealsense2-dbg | Debug symbols for developers  | librealsense2
librealsense2-gl | GLSL extension module runtime and configuration file | librealsense2
librealsense2-gl-dev | GLSL development header files and symbolic link | librealsense2
librealsense2-gl-dbg | GLSL debug symbols required for debugging purposes | librealsense2
librealsense2-net | Data over Ethernet extension module, runtime and configuration file | librealsense2 
librealsense2-net-dev | Network module developer's files | librealsense2 
librealsense2-net-dbg | Network module debug symbols | librealsense2

**Note** The packages include binaries and configuration files only.
Use the github repository to obtain the source code.

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

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
# After install
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

# Done


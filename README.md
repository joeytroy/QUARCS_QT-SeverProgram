# QUARCS_QT-ServerProgram

<img align="left" src="https://www.raspberrypi.com/app/uploads/2020/06/raspberrry_pi_logo.png" width="48">

These instructions are written for **Ubuntu 24.04.1 LTS Server (64-bit) ARM** on **Raspberry Pi 4 and 5**. All commands should be run via SSH on the Raspberry Pi connected to the internet. Please follow the steps carefully, as the setup process involves numerous commands and software builds.

---

## 1. OS Pre-requisites

### Update the OS
First, update and upgrade the operating system:

	sudo apt update && sudo apt full-upgrade -y && sudo reboot 


After rebooting, log back in via SSH. To check for any remaining updates, run:

	apt list --upgradable 


If updates are available, install them with:

	sudo apt install <package_name> -y 


For example:

	sudo apt install python3-distupgrade -y 


After confirming that all updates are complete, reboot the system again:

	sudo reboot now 

---

## 2.Install QUARCS Pre-requisites:
Install the required system dependencies by running:

	sudo apt-get install subversion build-essential cmake zlib1g-dev libgl1-mesa-dev libdrm-dev gcc g++ graphviz doxygen gettext git libxcb-xinerama0 gnome-keyring libusb-1.0.0-dev libcfitsio-dev astrometry.net astrometry-data-tycho2 wget libopencv-dev python-dev-is-python3 libgtk2.0-dev pkg-config libavcodec-dev python3-opencv libavformat-dev libswscale-dev libtbbmalloc2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-dev -y 

---

## 3. Install OPENCV 3.4.18
To install OpenCV 3.4.18, execute the following commands:

	cd ~ 
	wget https://github.com/opencv/opencv/archive/refs/tags/3.4.18.tar.gz 
	tar -xvzf 3.4.18.tar.gz 
	cd opencv-3.4.18/ 
	mkdir build 
	cd build 
	sudo cmake -D CMAKE_BUILD_TYPE=Release -D WITH_FFMPEG=0 -D CMAKE_INSTALL_PREFIX=/usr/local .. 
	sudo make -j$(nproc) 
	sudo make install 


**Verification**  
To confirm OpenCV installation, check the installed version:

	pkg-config opencv --modversion 

You should see **3.4.18** displayed in the terminal.

---

## 4. Install QHYCCD SDK
To download and install the QHYCCD SDK, run:

	cd ~ 
	wget https://www.qhyccd.com/file/repository/publish/SDK/240109/sdk_linux64_24.01.09.tgz 
	tar xvf sdk_linux64_24.01.09.tgz 
	cd sdk_linux64_24.01.09 
	sudo bash install.sh 

---

## 5. Install INDI and INDI-3rd party driver libraries
**Install INDI Prerequisites**  
Run the following command to install prerequisites for INDI:

	sudo apt-get install cdbs dkms fxload libev-dev libgps-dev libgsl-dev libraw-dev libusb-dev libftdi-dev libkrb5-dev libnova-dev libfftw3-dev librtlsdr-dev libgphoto2-dev libboost-regex-dev libcurl4-gnutls-dev libtheora-dev -y 


**Install INDI Core**  
To install the INDI Core software:

	cd ~ 
	mkdir -p ~/Projects 
	cd ~/Projects 
	git clone https://github.com/indilib/indi.git 
	mkdir -p ~/Projects/build/indi-core 
	cd ~/Projects/build/indi-core 
	cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Debug ~/Projects/indi 
	make -j$(nproc) 
	sudo make install 


**Install INDI 3rd-Party Prerequisites**  
Install additional libraries for INDI 3rd-party drivers:

	sudo apt-get install liblimesuite-dev libftdi1-dev libavdevice-dev libindi-dev -y 


**Install INDI 3rd-Party Drivers**  
Finally, install the INDI 3rd-party driver libraries:

	cd ~/Projects 
	git clone https://github.com/indilib/indi-3rdparty 
	mkdir -p ~/Projects/build/indi-3rdparty-libs 
	cd ~/Projects/build/indi-3rdparty-libs 
	cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Debug -DBUILD_LIBS=1 ~/Projects/indi-3rdparty 
	make -j$(nproc) 
	sudo make install 

---

## 6. Install QT components
**Install QT Prerequisites**  
Run the following command to install the required QT libraries:

	sudo apt install qtcreator qtbase5-dev qtscript5-dev libqt5svg5-dev qttools5-dev-tools qttools5-dev libqt5opengl5-dev qtmultimedia5-dev libqt5multimedia5-plugins libqt5serialport5 libqt5serialport5-dev qtpositioning5-dev libqt5positioning5 libqt5positioning5-plugins qtwebengine5-dev libqt5charts5-dev libqt5websockets5-dev libstellarsolver2 libstellarsolver-dev -y 


**Install QT Components**  
To build and install the QT components:

	cd ~ 
	git clone --branch RPi4_5 https://github.com/joeytroy/QUARCS_QT-SeverProgram 
	cd QUARCS_QT-SeverProgram/src/ 
	mkdir build 
	cd build 
	cmake .. 
	make -j$(nproc) 
	sudo make install 


This completes the installation of the **QUARCS_QT-ServerProgram**. To proceed further, you will need to install **QUARCS_phd2**.

---

## Need Assistance?
If you need assistance, join the **[QUARCS Discord Discussion Group](https://discord.gg/uHTPfJ5uuV)** for support and discussions.      

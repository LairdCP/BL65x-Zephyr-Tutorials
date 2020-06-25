# Getting Started with Zephyr on Ubuntu 20.04

This tutorial is based on the [Zephyr Getting Started Guide](https://docs.zephyrproject.org/latest/getting_started/index.html). The only difference is we added an extra step (i.e. see step 5 below) to install the nRF Command Line Tools.

This tutorial will show you how to setup the Zephyr development environment, build the Blinky app and flash the app into the BL65x dev kit.

1.  Update the OS

   ```
   sudo apt update
   sudo apt upgrade
   ```

   

2. Install Dependencies

   ```
   sudo apt install --no-install-recommends git cmake ninja-build gperf \
        ccache dfu-util device-tree-compiler wget \
        python3-dev python3-pip python3-setuptools python3-tk python3-wheel xz-utils file \
        make gcc gcc-multilib g++-multilib libsdl2-dev
        
   cmake --version
   ```

   

3. Get Zephyr and Install Python Dependencies

   - Install west, and make sure ~/.local/bin is on your PATH environment variable

     ```
     pip3 install --user -U west
     echo 'export PATH=~/.local/bin:"$PATH"' >> ~/.bashrc
     source ~/.bashrc
     ```

     

   - Get Zephyr Source Code

     ```
     west init ~/zephyrproject
     cd ~/zephyrproject
     west update
     ```

     

   - Export Zephyr CMake package

     ```
     west zephyr-export
     ```

     

   - Zephyr’s scripts/requirements.txt file declares additional Python dependencies. Install them with pip3

     ```
     pip3 install --user -r ~/zephyrproject/zephyr/scripts/requirements.txt
     ```

     

4. Install Toolchain

   - Download the latest SDK installer

     ```
     cd ~
     wget https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.11.3/zephyr-sdk-0.11.3-setup.run
     ```

     

   - Run the installer, installing the SDK in ~/zephyr-sdk-0.11.3

     ```
     chmod +x zephyr-sdk-0.11.3-setup.run
     ./zephyr-sdk-0.11.3-setup.run -- -d ~/zephyr-sdk-0.11.3
     ```

     

   - Install udev rules

     ```
     sudo cp ~/zephyr-sdk-0.11.3/sysroots/x86_64-pokysdk-linux/usr/share/openocd/contrib/60-openocd.rules /etc/udev/rules.d
     sudo udevadm control --reload
     ```

     

5. Install nRF Command Line Tools Pkg

   - Download Tools from Nordic Semiconductor Website

     https://www.nordicsemi.com/Software-and-Tools/Development-Tools/nRF-Command-Line-Tools/Download#in

   - Extract files into a directory of your choice

     

   - cd into the directory and install

     ```
     sudo apt-get install libncurses5
     sudo dpkg -i --force-overwrite JLink_Linux_V680a_x86_64.deb
     sudo dpkg -i --force-overwrite nRF-Command-Line-Tools_10_9_0_Linux-amd64.deb
     ```

     

6. Build Blinky

   - Build for BL652

     ```
     cd ~/zephyrproject/zephyr
     west build -p auto -b bl652_dvk samples/basic/blinky
     ```

     

   - Build for BL653

     ```
     Coming soon!
     ```

     

   - Build for BL654

     ```
     cd ~/zephyrproject/zephyr
     west build -p auto -b bl654_dvk samples/basic/blinky
     ```

     

7. Flash the build

   - Connect Ubuntu PC to USB2 port of the dev kit

   - Flash the build

     ```
     west flash
     ```

     

8. Verify LED1 is flashing
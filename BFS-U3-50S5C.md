# ----- page under construction -----
### BFS-U3-50S5C
Product page: [BFS-U3-50S5C-C](https://www.ptgrey.com/blackfly-s-color-50-mp-usb3-vision-sony-imx264)

#### Step I: Download Spinnaker SDK 1.13
1. Visit [FLIR File Downloads](https://www.ptgrey.com/support/downloads) and create a free account or log-in at the top right corner of the page.
2. Select `Blackfly S` under 'Product Families',
3. Select `All Models` under 'Camera Models',
4. Select `Linux Ubuntu 16.04` under 'Operating Systems'.
5. Once the filter is applied and results populate, click on 'Software' -> 'spinnaker-1.13.0.31-amd64-pkg.tar.gz'
6. If prompted, click 'Save As'.
7. Navigate to the 'Downloads' folder (/home/user1/Downloads), right click on the file you just downloaded and click 'Extract here'.

#### Step II: Dependency installation
1. Install dependencies: `sudo apt-get install libavcodec-ffmpeg56 libavformat-ffmpeg56 \libswscale-ffmpeg3 libswresample-ffmpeg1 libavutil-ffmpeg54 libusb-1.0-0`
2. Increase default image size limit from 2MiB to 1000MiB.
- `gksu gedit /etc/default/grub`
- Replace `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"` with `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash usbcore.usbfs_memory_mb=1000"`
- Update grub: `sudo update-grub`
- Run the following command: `sudo sh -c 'echo 1000 > /sys/module/usbcore/parameters/usbfs_memory_mb'`
- Reboot computer: `sudo reboot`

#### Step III: Download and Install firmware
1. Visit [FLIR File Downloads](https://www.ptgrey.com/support/downloads)
2. Select `Blackfly S` under 'Product Families',
3. Select `All Models` under 'Camera Models',
4. Select `Linux Ubuntu 16.04` under 'Operating Systems'.
5. Once the filter is applied and results populate, click on 'Firmware' -> 'BFS-U3-50S5_1803.0.167.2'
6. If prompted, click 'Save As'.
7. Navigate to the 'Downloads' folder (/home/user1/Downloads), right click on the file you just downloaded and click 'Extract here'.

#### Step IV: Install Spinnaker SDK 1.13
1. Navigate to the folder where you extracted the SDK files in Step I.7 above: `cd ~/Downloads/spinnaker-1.13.0.31-amd64`
2. Run the install script: `sudo sh install_spinnaker.sh`. Answer 'yes' `y` to all the prompts during the install process.
3. Once complete, connect the camera to a USB3.0 port and test installation by typing `spinview` in the terminal. If Spinnaker SDK was successfully installed, a GUI will open with a list of cameras detected. Click on the BFS camera and then on the green play button to view live feed. Make note of the camera's Serial number (Ex: 18284612).

#### Step V: Install ROS Wrapper for Spinnaker SDK [ref](http://wiki.ros.org/spinnaker_sdk_camera_driver)
1. Run command: `sudo apt install libunwind-dev ros-kinetic-cv-bridge ros-kinetic-image-transport`
2. Navigate to the src folder of your catkin workspace: `cd ~/catkin_ws/src`
3. Clone this repository: `git clone https://github.com/neufieldrobotics/spinnaker_sdk_camera_driver.git`
4. Step back into catkin workspace `cd ~/catkin_ws` and then compile `catkin_make`
5. Use the serial number obtained from Step IV.3 to edit the test_params.yaml file: `cd /home/user1/catkin_ws/src/spinnaker_sdk_camera_driver/params` and `gedit test_params.yaml`. Fill in the cam_id and master_cam with the serial number, save and close file.
6. In one terminal ,launch: `roslaunch spinnaker_camera_driver acquisition.launch`
7. Open a second terminal tab for rqt: `rosrun rqt_image_view rqt_image_view` and select the 'acquisition' topic from the drop down menu to view the live feed.

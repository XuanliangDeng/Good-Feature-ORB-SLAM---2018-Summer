# Pose estimation with ROS and ArUco marker
This is the project for Summer 2018, supervised by Professor Patricio A. Vela &amp; Yipu Zhao

# Description
There is nothing fancy here, just want to write down my experience to remind me of what I have done. Hope this could be helpful:) 
All related links &amp; sources are attached. Thank ros-developer for help.  [ROS-Developer](http://ros-developer.com/2017/04/23/aruco-ros/)

## Device Check
When you plug in your USB camera, check the device information in your ubuntu system by typing
```C++
$ cd /dev
$ l
```
If your device connection has no problem, you may see both video0 &amp; video1. For me, the video0 is the webcam of my macbook, while video1 is the USB camera I want to use.


## Camera Calibration
To detect ArUco markers using usb cameras, the first step is camera calibration. I used [camera_calibration](http://wiki.ros.org/camera_calibration) , a ros package, to implement this. To be more specific, my USB camera is [UC20MPD_ND](https://store.spinelelectronics.com/UC20MPD_ND)

* Install
```C++
$ roscore
$ rosdep install camera_calibration
$ sudo apt-get install ros-indigo-usb-cam ros-indigo-aruco-ros
```

* Callibration

Run the following code and use rostopic list to check whether the related topics are published.
```C++
$ source /opt/ros/indigo/setup.bash
$ roslaunch usb_cam usb_cam-text.launch
$ rostopic list
```
If everything is fine, you should see these two topics: /usb_cam/camera_info &amp; /usb_cam/image_raw. You should also see some warnings about head_camera.yaml, don't worry we are gonna fix these after calibration.

The next step is to calibrate the camera using the python file, the window will become grey when computing it's normal don't be worry. For this camera, I collected 100 samples for calibration from different viewpoints and size. I print the chessboard on a A4 sheet so each square is 2.4cm, converted into meters.

```C++
$ rosrun camera_calibration cameracalibrator.py --size 8x6 --square 0.024  image:=/usb_cam/image_raw camera:=/usb_cam
```
When it is done, click commit, the head_camera.yaml file will be saved.
One result could be like this：
```C++
('D = ', [0.02769347087200823, -0.001400754024811914, -0.004704394200188205, -0.0018190502592750099, 0.0])
('K = ', [477.00761304970223, 0.0, 308.4895022533115, 0.0, 479.5071943003428, 218.6693607752001, 0.0, 0.0, 1.0])
('R = ', [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0])
('P = ', [486.17041015625, 0.0, 306.9950571527006, 0.0, 0.0, 487.277587890625, 215.99803311404685, 0.0, 0.0, 0.0, 1.0, 0.0])
None
# oST version 5.0 parameters

[image]

width
640

height
480

[narrow_stereo]

camera matrix
477.007613 0.000000 308.489502
0.000000 479.507194 218.669361
0.000000 0.000000 1.000000

distortion
0.027693 -0.001401 -0.004704 -0.001819 0.000000

rectification
1.000000 0.000000 0.000000
0.000000 1.000000 0.000000
0.000000 0.000000 1.000000

projection
486.170410 0.000000 306.995057 0.000000
0.000000 487.277588 215.998033 0.000000
0.000000 0.000000 1.000000 0.000000
```
You should see results like this pops up in the terminal, telling you that the yaml file has been created.
```c++
[ INFO] [1529361043.469128729]: writing calibration data to /home/dengxuanliang/.ros/camera_info/head_camera.yaml
```
Congrats! The calibration is complete, for more methods, please check the link.

## ArUco marker detection
This part is about AR marker detection, please check the src directory for all the launch files I used.
* Publish images from USB camera:

Before publish this information, REMEMBER TO ABORT the process in previous steps(roslaunch usb_cam usb_cam-text.launch). Otherwise it will pop up an error saying that device is busy.

```c++
$ cd catkin_ws/src
$ roslaunch usb_cam_stream_publisher.launch
```
* ArUco marker detection &amp; 6DOF pose 

```c++
$ cd catkin_ws/src
$ roslaunch aruco_marker_finder.launch markerId:=701 markerSize:=0.074
```

* Visualization
Use the rqt gui to visualize the marker detection result, remember to add plugin. Menu -> Plugins -> Visualization -> Image_View. To generate ArUco marker, you can use the following website: [ArUco marker generation](https://terpconnect.umd.edu/~jwelsh12/enes100/markergen.html)

```
$ rosrun rqt_gui rqt_gui
```

* Print pose estimation

```
$ rostopic echo /aruco_single/pose
```
You will see both transformation and rotation information in the screen.


# Camera pose estimation with ROS and ChArUco markers
This is summer project 2018, supervised by Professor Patricio A. Vela &amp; Yipu Zhao
The main tools are OpenCV, ChArUco, ROS. 

## Device description
The drone I use for this project is Parrot AR Drone 2.0. The ROS version is Kinetic with OpenCV 3.3.1(including OpenCV_contrib). ChArUco stands for Chessboard + ArUco = ChArUco. The functions related to ChArUco marker detection are already included in OpenCV(after 3.0 version). 

## Software dependencies
* ROS - tested with Kinetic version [ROS Kinetic Installation](http://wiki.ros.org/kinetic/Installation/Ubuntu)
* OpenCV - tested with OpenCV 3.3.1 with corresponding opencv_contrib
* For opencv_contrib and OpenCV3 installation, these instructions on this website could be very helpful(could skip those steps about virtual environment)   [OpenCV Installation](https://www.learnopencv.com/install-opencv3-on-ubuntu/)



## Camera calibration
Before any tests, the first key step is to calibrate the camera we use. The function for this is included in directory and the file name is calibrate_camera_charuco.cpp.
```C++
$ cd ~/opencv_contrib/modules/aruco/samples/
```











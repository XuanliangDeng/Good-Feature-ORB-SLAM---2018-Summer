# Good-Feature-ORB-SLAM---2018-Summer
This is the project for Summer 2018, supervised by Professor Patricio A. Vela &amp; Yipu Zhao

# Description
There is nothing fancy here, just want to write down my experience to remind me of what I have done. Hope this could helpful:) 
All related links &amp; sources are attached.

## Camera Calibration
To detect ArUco markers using usb cameras, the first step is camera calibration. I used [camera_calibration](http://wiki.ros.org/camera_calibration) , a ros package, to implement this. To be more specific, my USB camera is [UC20MPD_ND](https://store.spinelelectronics.com/UC20MPD_ND)

* Install
```C++
$ rosdep install camera_calibration
$ sudo apt-get install ros-indigo-usb-cam
```

* Callibration

Run the following code and use rostopic list to check whether the related topics are published.
```C++
$ roslaunch usb_cam usb_cam-text.launch
$ rostopic list
```
If everything is fine, you should see these two topics: /usb_cam/camera_info &amp; /usb_cam/image_raw. You should also see some warnings about head_camera.yaml, don't worry we are gonna fix these after calibration.




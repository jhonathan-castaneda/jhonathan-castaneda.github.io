---
layout: post
title: Stereo image using monocular cameras and ros
---

In this practical tutorial i explain how to configure a pair of monocular cameras to use them like a stereo camera and obtain 3D point clouds. The required material for this tutorial is 2 usb cameras, an installation of ros in your computer and a chessboard template for the calibration of the cameras. 

In order to follow the proposed steps in this tutorial you will need to have installed the next ros packages, in my case i am using ros melodic in Ubuntu 18.04.

* usb_cam
* camera_calibration
* image_view
* stereo_image_proc
* dynamic_reconfigure
* rviz

Also, you need to download and print this template.

<a id="raw-url" href="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/_download/calibration_template.pdf">Calibration template</a>

And download the next files.

For this tutorial I'm using a pair of logitech c920 cameras.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/1.png" width="50%" height="50%">
<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/1.png">


you can use any other usb webcam as well or another type of camera if you are allowed to connect them to your computer via usb. By first, let's identify the right camera and the left one, this is because we need to distinguish each one in our system for making the calibration and then set up the stereo image parameters.

For this, before plugging in the usb cameras, open a terminal in your system and type the next command.

![2](https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/2.png)

Run it and you will see something like this.

![3](https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/3.png)

This pair of devices correspond to a single physical device, in this case my integrated computer camera, from the first one you can get the video data, the second one just gives information about the device.





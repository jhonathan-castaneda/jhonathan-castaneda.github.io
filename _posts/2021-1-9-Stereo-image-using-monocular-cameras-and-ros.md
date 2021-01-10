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

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/1.png" width="30%" height="30%">

you can use any other usb webcam as well or another type of camera if you are allowed to connect them to your computer via usb. By first, let's identify the right camera and the left one, this is because we need to distinguish each one in our system for making the calibration and then set up the stereo image parameters.

For this, before plugging in the usb cameras, open a terminal in your system and type the next command.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/2.png" width="45%" height="45%">

Run it and you will see something like this.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/3.png" width="45%" height="45%">

This pair of devices correspond to a single physical device, in this case my integrated computer camera, from the first one you can get the video data, the second one just gives information about the device.

Now, plug in the left usb camera on your computer, you will see how the device appear in the console.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/4.png" width="45%" height="45%">

Once again we get a pair of devices, but video2 is our left image channel. Connect the right camera and you will see the last pair of devices.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/5.png" width="45%" height="45%">

Now we have the device name of the left camera and the right camera, video2 and video4 respectively (these will probably be different for you), with this info set up the launch file for the calibration.

go to the directory when you have the “calibration.launch” file, and edit the source for the left and the right image using the names that you got with the previous steps.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/6.png" width="45%" height="45%">
<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/7.png" width="45%" height="45%">

This previous launch file sets main parameters for the usb cameras as for example image resolution, frames per second for video, pixel format and focal length, also establishes the topics where the video data will be published for each image source. You can edit the value for each parameter depending on your preferences.




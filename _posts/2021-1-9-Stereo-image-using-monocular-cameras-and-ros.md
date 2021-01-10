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

[1](https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/config.png)

you can use any other usb webcam as well or another type of camera if you are allowed to connect them to your computer via usb. By first, let's identify the right camera and the left one, this is because we need to distinguish each one in our system for making the calibration and then set up the stereo image parameters.



The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.

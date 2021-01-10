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

[Calibration template](https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/calibration_template.pdf)

And download the next files.



![1]({{ site.baseurl }}/images/1.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.

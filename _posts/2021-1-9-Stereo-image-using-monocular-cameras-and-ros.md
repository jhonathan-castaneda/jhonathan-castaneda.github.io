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

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/6.png" width="75%" height="75%">
<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/7.png" width="75%" height="75%">

This previous launch file sets main parameters for the usb cameras as for example image resolution, frames per second for video, pixel format and focal length, also establishes the topics where the video data will be published for each image source. You can edit the value for each parameter depending on your preferences.

With this configuration you can run the stereo calibration using the camera_calibration package from Ros. In order to make this, take your chessboard template for calibration and measure the size of the squares and check the number of intersections between black and withe squares in horizontal and vertical direction.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/8.png" width="35%" height="35%">

In my case the squares have 0.0244 m in each side approximately and the intersections are 9 and 6 respectively. With this information, run the calibration.launch file and then in another terminal run the calibration node.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/9.png" width="45%" height="45%">

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/10.png" width="45%" height="45%">

Note that in the calibration node you need to specify the size of the squares in your chessboard template and the previous mentioned chessboard intersections (9x6 in my case). Also you need to specify the topic where each source of image is.

The calibration window will appear and you will be able to start the process.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/11.png" width="75%" height="75%">

As soon as you start moving the calibration template in the camera's field of view the samples for calibration will be taken, move the chessboard around, then when the marks of X. Y. Skew and Size are green, you can stop the sampling and calibrate the cameras.
When you select the option “calibrate” the system will start to estimate a group of vectors with information about your cameras, specifically the distortion of each image, the camera matrix based on the pinhole model, the rectification matrix for each camera and the projection matrix of these too, this process takes some of time depending on the capabilities of each computer.

Wait for a while and then when the process is completed you will see some information in your console.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/12.png" width="75%" height="75%">

Here, you can find the camera matrix (K) and distortion matrix (D), also the rectification, matrix (R) and the projection matrix (P). The previous image for example shows just the information of one of my cameras, but you will get the information from both of them of course.

Copy this information from the console in a text file and save it, now, in a .yml file with the next structure write down the values of K, D, R, and P for the right camera, specify the resolution used in the calibration process (in my case it was 1280x720p.) and then just save the file with the name right.yaml, repeat this for the left camera information and save the respective file as left.yaml. At the end you will get 2 .yaml files with the information from your pair of cameras. These .yaml files will be used later by the block matching algorithm to get the disparity map from the stereo image.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/13.png" width="45%" height="45%">

Up to here, there is just one last step before you can run the stereo to get point clouds of your environment, the run_stereo.launch file configuration.

Open the run_stereo.launch file and edit the camera_info_url parameter, write down there the path for the data of each camera, this path is where you have saved previously the pair of .yaml files with the information of the calibration.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/14.png" width="75%" height="75%">
<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/15.png" width="75%" height="75%">

save it and then launch it, after this, the disparity map will appear.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/16.png" width="75%" height="75%">

The last picture of the disparity map is from a test into a room, you can test the stereo outside and check the generated point cloud in rviz too, but I recommend you to protect the cameras with uv-ir filters over the camera’s lenses (if you don’t have one of these you can use a little piece of polarized car film over the lenses to protect them temporally) this is because some usb cameras are specifically designed for indoor usage, where the exposure to ir and uv light is very low compared with the exposition outside under common daylight illumination.

If you use your usb cameras outside without any protection the image can be damaged, there is an example of this below.

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/17.png" width="75%" height="75%">

On the right side we can see how the image is damaged when we don’t use any protection for the usb cameras. In the left side there is an approximation of how the image should be, this approximation was obtained by applying an adjustment to the damaged image with basic image processing techniques. This looks bad, but the worst part about this is the heating of the CCD or CMOS sensor by the exposure to the daylight illuminations without any protection, If you don't protect the lenses the overheating in the sensor will burn it. 

As I mentioned before if you open rviz, after you run the previous launch file, you will be able to check the generated point cloud, here we have an example of the final result where the stereo was capable to generate a point cloud of objects that are located in a large range (up to 50 meters approximately)

<img src="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/images/18.png" width="75%" height="75%">

If the disparity map and of course the point cloud are not showing good results, just adjust the stereo processing parameters, you can check them with their meaning and operation ranges in **[choosing good stereo parameters]**(http://wiki.ros.org/stereo_image_proc/Tutorials/ChoosingGoodStereoParameters
) ,  for example. If you want to set these parameters, open another terminal and launch the dynparam_reconfigure node of Ros.















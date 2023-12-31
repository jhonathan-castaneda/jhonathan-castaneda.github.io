---
layout: post
title: Stereo image using monocular cameras and ros
---

<p style="text-align: justify">
In this practical tutorial I explain how to configure a pair of monocular cameras to use them like a stereo camera and obtain 3D point clouds. The required material for this tutorial is 2 usb cameras, an installation of ros in your computer and a chessboard template for the calibration of the cameras. This practical guide is based on the next material: <a href="http://wiki.ros.org/stereo_image_proc">stereo image proc</a>, <a href="http://ros-developer.com/2018/01/18/stereo-camera-calibration-with-ros-and-opencv/#comments">Stereo Camera Calibration with ROS and OpenCV</a> and <a href="https://docs.opencv.org/master/dc/dbb/tutorial_py_calibration.html">camera calibration</a>.


</p>

<p style="text-align: justify">
In order to follow the proposed steps in this tutorial you will need to have installed the next ros packages, in my case I'm using ros melodic in Ubuntu 18.04.
</p>

* usb_cam
* camera_calibration
* image_view
* stereo_image_proc
* dynamic_reconfigure
* rviz

<p style="text-align: left">
Also, you need to download and print this template.
</p>

* <a id="raw-url" href="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/_download/calibration_template.pdf">Calibration template</a>

<p style="text-align: left">
And download the next files.
</p>

* <a id="raw-url" href="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/_download/camera_info_template.yaml">camera_info_template.yaml</a>

* <a id="raw-url" href="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/_download/calibration.launch">calibration.launch</a>

* <a id="raw-url" href="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/_download/run_stereo.launch">run_stereo.launch</a>

<p style="text-align: left">
Move the last two .launch files listed above to the launch directory of the usb_cam package, after this, we can get started with the tutorial.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/1.jpg" width="40%" height="40%">
</div>

<p style="text-align: justify">
I'm using a pair of logitech c920 cameras for this tutorial, but you can use any other usb webcam as well or another type of camera if you can connect them to your computer via usb. First of all, let's identify the right camera and the left one, this is because we need to distinguish each one in our system for making the calibration and then set up the stereo image parameters.
</p>

<p style="text-align: justify">
For this, before plugging in the usb cameras, open a terminal in your system and type the next command.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/2.jpg" width="45%" height="45%">
</div>

<p style="text-align: justify">
Run it and you will see something like this.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/3.jpg" width="45%" height="45%">
</div>

<p style="text-align: justify">
This pair of devices corresponds to a single physical device, in this case my integrated computer's camera, from the first one you can get the video data, the second one just gives information about the device.
</p>

<p style="text-align: justify">
Now, plug in the left usb camera on your computer, you will see how the device appear in the console.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/4.jpg" width="45%" height="45%">
</div>

<p style="text-align: justify">
Once again we get a pair of devices, but video2 is our left image channel. Connect the right camera and you will see the last pair of devices.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/5.jpg" width="45%" height="45%">
</div>

<p style="text-align: justify">
Now we have the device name of the left camera and the right camera, video2 and video4 respectively (these will probably be different for you), with this info set up the launch file for the calibration.
</p>

<p style="text-align: justify">
Go to the directory when you have the <a href="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/_download/calibration.launch">calibration.launch</a> file, and edit the source for the left and the right image using the names that you got with the previous steps.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/6.jpg" width="75%" height="75%">
  <img src="{{site.baseurl}}/images/7.jpg" width="75%" height="75%">
</div>

<p style="text-align: justify">
This previous launch file sets some of the main parameters for the usb cameras, for example the image resolution, frames per second for video, pixel format and focal length, also establishes the topics where the video data will be published for each image source. You can edit the value for each parameter depending on your preferences.
</p>

<p style="text-align: justify">
With this configuration you can run the stereo calibration using the camera_calibration package from ros. In order to make this, take your chessboard template for calibration and measure the size of the squares and check the number of intersections between black and withe squares in horizontal and vertical direction.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/8.jpg" width="35%" height="35%">
</div>

<p style="text-align: justify">
In my case the squares have 0.0244 m in each side approximately and the intersections are 9 and 6 respectively. With this information, run the calibration.launch file and then in another terminal run the calibration node.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/9.jpg" width="45%" height="45%">
</div>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/10.jpg" width="45%" height="45%">
</div>

<p style="text-align: justify">
Note that in the calibration node you need to specify the size of the squares in your chessboard template and the previous mentioned chessboard intersections (9x6 in my case). Also you need to specify the topic where each source of image is.
</p>

<p style="text-align: justify">
A new window will appear and you will be able to start the calibration process.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/11.jpg" width="75%" height="75%">
</div>

<p style="text-align: justify">
As soon as you start moving the calibration template in the camera's field of view the samples for calibration will be taken, move the chessboard around, then when the marks of X. Y. Skew and Size are green, you can stop the sampling and calibrate the cameras.
When you select the option “calibrate” the system will start to estimate a group of vectors with information about your cameras, specifically the distortion of each image, the camera matrix based on the pinhole model, the rectification matrix for each camera and the projection matrix of these too, this process takes some of time depending on the capabilities of each computer.
</p>

<p style="text-align: justify">
Wait for a while and then when the process is completed you will see some information in your console.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/12.jpg" width="75%" height="75%">
</div>

<p style="text-align: justify">
Here, you can find the camera matrix (K) and distortion matrix (D), also the rectification, matrix (R) and the projection matrix (P). The previous image for example shows just the information of one of my cameras, but you will get the information from both of them of course.
</p>

<p style="text-align: justify">
Copy this information from the console in a text file and save it, now, in the <a href="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/_download/camera_info_template.yaml">camera info template</a> write down the values of K, D, R, and P for the right camera, specify the resolution used in the calibration process (in my case it was 1280x720p.) and then just save the file with the name right.yaml, repeat this for the left camera information and save the respective file as left.yaml. At the end you will get 2 .yaml files with the information from your pair of cameras. These .yaml files will be used later by the block matching algorithm to get the disparity map from the stereo image.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/13.jpg" width="45%" height="45%">
</div>

<p style="text-align: justify">
Up to here, there is just one last step before you can run the stereo image processing to get point clouds of your environment, the run_stereo.launch file configuration.
</p>

<p style="text-align: justify">
Open the <a href="https://github.com/jhonathan-castaneda/jhonathan-castaneda.github.io/blob/master/_download/run_stereo.launch">run_stereo.launch</a> file and edit the camera_info_url parameter, write down there the path for the data of each camera, this path is where you have saved previously the pair of .yaml files with the information of the calibration.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/14.jpg" width="75%" height="75%">
  <img src="{{site.baseurl}}/images/15.jpg" width="75%" height="75%">
</div>

<p style="text-align: justify">
Save it and then launch it, after this, the disparity map will appear.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/16.jpg" width="75%" height="75%">
</div>

<p style="text-align: justify">
The last picture of the disparity map is from a test into a room, you can test the stereo outside and check the generated point cloud in rviz too, but I recommend you to protect the cameras with uv-ir filters over the camera’s lenses (if you don’t have one of these you can use a little piece of polarized car film over the lenses to protect them temporally) this is because some usb cameras are specifically designed for indoor usage, where the exposure to ir and uv light is very low compared with the exposition outside under common daylight illumination.
</p>

<p style="text-align: justify">
If you use your usb cameras outside without any protection the image can be damaged, there is an example of this below.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/17.jpg" width="75%" height="75%">
</div>

<p style="text-align: justify">
On the right side we can see how the image is damaged when we don’t use any protection for the usb cameras. In the left side there is an approximation of how the image should be, this approximation was obtained by applying an adjustment to the damaged image with basic image processing techniques. This looks bad, but the worst part about this is the heating of the CCD or CMOS sensor by the exposure to the daylight illuminations without any protection, If you don't protect the lenses the overheating in the sensor will burn it. 
</p>

<p style="text-align: justify">
As I mentioned before if you open rviz, after you run the previous launch file, you will be able to check the generated point cloud, here we have an example of the final result where the stereo was capable to generate a point cloud of objects that are located in a large range (up to 50 meters approximately)
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/18.jpg" width="75%" height="75%">
</div>

<p style="text-align: justify">
If the disparity map and of course the point cloud are not showing good results, just adjust the stereo processing parameters, you can check them with their meaning and operation ranges in <a href="http://wiki.ros.org/stereo_image_proc/Tutorials/ChoosingGoodStereoParameters">Choosing Good Stereo Parameters</a>
. If you want to set these parameters, open another terminal and launch the dynparam_reconfigure node of ros.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/19.jpg" width="45%" height="45%">
</div>

<p style="text-align: justify">
In the previous image for example, i am setting the values for the stereo parameters, I am using these values for processing the stereo image with my cameras but you can test with other values and check how the disparity map and the pointcloud change.
</p>

<p style="text-align: justify">
As I mentioned before, you can use any other type of usb cameras, for example I have also used a pair of IR cameras, these have aviator cable output instead of usb but you can adapt them to work as usb cameras using a rca to usb adapter.
</p>


<div style="text-align: center">
  <img src="{{site.baseurl}}/images/20.jpg" width="45%" height="45%"> 
</div>


<p style="text-align: justify">
A nice thing about this type of cameras is the increased field of view compared to the one that we have with the pair of logitech cameras where we get around 78°. the field of view for these IR cameras is 120° approximately, the only problem with this cameras is the high distortion in the lenses, but don’t be worry about this, with a good calibration you can correct the effect of this on the images. With these cameras you will be able to get point clouds even at night using the IR illumination, also you don’t need to use filters like the ones we mentioned before for the usb cameras because these cameras are designed for outdoor usement. Here is an example of a point cloud obtained with the IR cameras.
</p>

<div style="text-align: center">
  <img src="{{site.baseurl}}/images/21.jpg" width="75%" height="75%">
</div>

<p style="text-align: justify">
The resolution is smaller of course, and there are some occlusion areas, but with a better calibration you will get better results. An important thing about using these IR cameras is that if you are going to use them, you need to modify the video format in the launch files for calibration and stereo image processing, use mjpeg format instead of using yuyv and adjust the image resolution to a compatible one with the cameras.
</p>


















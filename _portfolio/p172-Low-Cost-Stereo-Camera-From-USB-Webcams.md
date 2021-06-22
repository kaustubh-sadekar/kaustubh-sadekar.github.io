---
title: "Low-Cost Custom Stereo Camera From USB Webcams"
excerpt: "Using two USB webcams a low cost stereo camera can be developed for depth perception<br/><img src='/images/stereo3D.gif'>"
collection: portfolio
feature_row:
  - image_path: ../images/capturing_stereo.gif
    excerpt: "Process of capturing stereo images for 3D reconstruction"
  - image_path: ../images/stereo_gif.gif
    excerpt: "Stereo image pairs with corresponding disparity maps in the middle"
  - image_path: ../images/pointcloud.gif
    excerpt: "2.5D Pointcloud generated using one of the stereo image pair after post processing"
---

Project in brief
================

<a href="https://en.wikipedia.org/wiki/Stereo_camera" target="_blank">Stereo cameras</a> are a standard solution for depth perception. Most industrial-grade stereo cameras come with well written APIs and SDKs to make it easy for the developers to get the depth data from the camera with just a few lines of code. However, these cameras are quite expensive compared to the regular USB cameras.

I learned the fundamental concepts of stereo vision from CS 763 - Computer Vision at IIT Bombay (sit through). This motivated me to develop a custom low-cost stereo camera using two USB cameras and write a custom software library that. The software is written in Python as well as C++.

{% include feature_row %}


Design and Fabrication
----------------------

A 3D printed box was used to ensure that the cameras do not move and give it a nice look! My childhood friend and neighbor [**Mandar Kadwekar**](https://www.linkedin.com/in/mandar-kadwekar-19706a170/) designed the
 3d box. He is very proficient in 3D designing.

<p align="center">
  <img src='/images/cameraDesign.gif'>
</p>
<p align="center">
  Stages of StereoCam setup
</p>

Stereo Calibration
------------------

I created a **custom stereo calibration grid using ArUco markers**. It is easier to detect compared to the checkerboard pattern. The marker corner detection is done using ArUco class in OpenCV.

<p align="center">
  <img src='/images/calibration.gif'>
</p>
<p align="center">
  Capturing images for calibration using the custom calibration grid
</p>


Depth Estimation and 2.5 D Projection
-------------------------------------

Each pixel's depth is estimated using the depth map and the calibrated camera parameters, and a 3D point cloud is generated. This is usually called a 2.5 D projection.
StereoSGBM method is used to estimate the disparity map, and the WLS filter is used to refine the disparity map. The disparity map information and intrinsic camera parameters 
are used to get the 2.5 D representation.

The point cloud generated in the above process is displayed using Open3D.

<p align="center">
  <img src='/images/stereo3D.gif'>
</p>
<p align="center">
  Live pointcloud displayed using Open3D
</p>





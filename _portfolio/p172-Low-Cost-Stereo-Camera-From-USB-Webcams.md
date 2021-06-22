---
title: "Low-Cost RGB-D Camera For 3D Perception"
excerpt: "Creating a low-cost RGB-D Camera under 80 USD that can be used for scanning and reconstruction of 3D objects.<br/><img src='/images/stereo3D.gif'>"
collection: portfolio
feature_row:
  - image_path: ../images/capturing_stereo.gif
    excerpt: "Process of capturing stereo images for 3D reconstruction"
  - image_path: ../images/pointcloud.gif
    excerpt: "2.5D Pointcloud generated using one of the stereo image pair after post processing"
---

## Project in brief

<a href="https://en.wikipedia.org/wiki/Stereo_camera" target="_blank">Stereo cameras</a> are a standard solutions for depth perception. However, these cameras are quite expensive compared to the regular USB cameras. The aim of this project is to create a custom RGB-D camera under 80 Dollars that can be used for scanning and reconstruction of 3D objects. The projet is divided into two major parts:

1. Design and Fabrication of the stereo camera setup.
2. Devloping a software to capture accurate RGB-D frames with the setup.

<p align="center">
  <img src='/images/stereo_gif.gif'>
</p>
<p align="center">
  Stereo image pairs with corresponding disparity maps in the middle captured using the low-cost stereo camera for 3D reconstruction.
</p>


## Design and Fabrication
---

A 3D printed casing was used to keep the cameras rigidly fixed with respect to each other. The casing was designed by [**Mandar Kadwekar**](https://www.linkedin.com/in/mandar-kadwekar-19706a170/). The casing is designed such that the imaging planes of the left and right cameras are almost parallel and horrizontally alligned. To know more about the design and fabrication steps please refer to my post - <a href="https://learnopencv.com/making-a-low-cost-stereo-camera-using-opencv/" target="_blank"> Making A Low-Cost Stereo Camera Using OpenCV</a>. 

<p align="center">
  <img src='/images/cameraDesign.gif'>
</p>
<p align="center">
  Stages of StereoCam setup.
</p>

## StereoCam Library
---

Fixing the cameras rigidly in a casing is not enough to make a stereo camera work. We need to process the stereo image frames captured by the cameras to obtain accurate depth information. I worte a library called **StereoCam** for this. It contains various functionalities to use such stereo camera setup for extracting RGB-D frames. Some of the key steps involved in extracting depth information using the StereoCam libarary are explained below. 

### Stereo Calibration And Rectification

I created a **custom stereo calibration grid using ArUco markers**. This pattern is easier to detect compared to the checkerboard pattern. The marker detection is done using the <a href="https://docs.opencv.org/3.4/d5/dae/tutorial_aruco_detection.html" target="_blank">ArUco class</a> available in OpenCV library. **StereoCam** contains a utility method for capturing and detecting the calibration pattern with just a few lines of code.

<p align="center">
  <img src='/images/calibration.gif'>
</p>
<p align="center">
  Capturing images of the calibration grid for stereo calibration.
</p>

### Generating Disparity Map Using Block Matching Algorithm

The **StereoCam** library has a facility to use the Block Matching algorithm for calculating dense stereo correspondence and disparity map. The disparity map is then used for calculation of depth maps. To learn more about disparity map and block matching algorithm please refer to my post <a href="https://learnopencv.com/depth-perception-using-stereo-camera-python-c/" target"_blank">Depth perception using stereo camera</a>. The post also explains how to convert disparity map to depth map. The Block Matching algorithm has several parameters that need to be tuned. **StereoCam** provides an interractive GUI to tune these parameters.

<p align="center">
  <img src='/images/tunedepthParams.gif'>
</p>
<p align="center">
   GIF showing the process of fine-tuning the parameters Using StereoCam GUI.
</p>


### From disparity map to depth map

Disparity and depth are inversely related. An easy way to find an accurate mapping between them is by collecting pairs of depth and disparity values and find the best fitting curve using least squares fitting.

<p align="center">
  <img src='/images/disparity2depth.gif'>
</p>
<p align="center">
  Capturing depth and disparity data points for curve fitting.
</p>


### Depth Estimation and 2.5 D Projection

Each pixel's depth is estimated using the depth map and the calibrated camera parameters, and a 3D point cloud is generated. This is usually called a 2.5 D projection. **StereoSGBM** method is used to estimate the disparity map, and the **WLS filter** is used to refine the disparity map. The depth map information and intrinsic camera parameters are used to get the 2.5 D representation using **backprojection equation**.

The point cloud generated in the above process is displayed using Open3D.

<p align="center">
  <img src='/images/stereo3D.gif'>
</p>
<p align="center">
  Live pointcloud displayed using Open3D
</p>


## Applications Developed Using The Low-Cost Stereo Camera
---

### Capturing RGB-D Data For 3D Reconstruction

A single RGB-D image can be used to generated a 2.5 D pointcloud using backprojection. If we capture RGB-D images of an object from multiple view points we can reconstruct a 3D model of the object by alligning all the individual 2.5 D pointclouds using methods like ICP - Iterative Closest Point algorithm. 

{% include feature_row %}


### Generating Anaglyph 3D Movies

We use stereopsis to perceive depth using our binocular vision system. We can simulate such disparities by artificially presenting two different images separately to each eye using a method called stereoscopy. Initially, for 3D movies, people achieved by encoding each eye’s image using filters of red and cyan colors. They used the red-cyan 3D glasses to ensure that each of the two images reached the intended eye. This created the illusion of depth. The stereoscopic effect generated with this method is called anaglyph 3D.

<p align="center">
  <img src='/images/Anaglyph3D.gif'>
</p>
<p align="center">
  Anaglyph 3D movie created using stereo camera setup
</p>


### References

I learned the fundamental concepts of stereo vision from <a href="https://www.cse.iitb.ac.in/~sharat/current/cs763/" target="_blank">CS 763 - Computer Vision at IIT Bombay (sit through)</a>. This motivated me to develop the custom low-cost stereo camera.



---
title: "Implementation of Kinect Fusion Algorithm For 3D Reconstruction"
excerpt: "Kinect Fusion implementation with GPU acceleration capabilities using PyTorch. Vectorized implementation of the ICP algorithm and TSDF Fusion algorithm to enable GPU aceleration capabilities.<br/><img src='/images/KinFu_office.gif'>"
collection: portfolio
---

## Project in brief
---

Kinect Fusion is a real-time 3D reconstruction algorithm that generates a 3D model of a given scene using a sequence of depth images. There are some standard opensource implementations for the Kinect Fusion algorithm available online. However, most of the are written in C++. This project aims to create a complete python implementation of the Kinect Fusion algorithm and use vectorization with PyTorch to enable GPU acceleration capabilities.


## Key Steps Of The Kinect Fusion Algorithm
---

1. Capture depth map of the scene.
2. Apply bilateral filtering to the depth map.
3. Obtain 2.5D representation (vertex map/ point cloud) by back-projecting depth map to 3D coordinate space.
4. Obtain normal vector for each projected point p by computing cross product (p1 - p)x(p2 - p). Where p1 and p2 are the neighbouring vertices.
5. Store the depth map information by reconstructing the surface using the Truncated Signed Distance Function (TSDF) for MxMxM voxel grid representing a KxKxK m3 space in front of the camera. (Store the TSDF values for each voxel in the voxel grid)
6. Capture the next depth map and apply steps 2,3 and 4 on it. Let the 2.5D pointcloud obtained be pcl_i and normal map obtained be nmap_i.
7. Using ray casting, obtain global pointcloud pcl_global and global normal map nmap_global. 
8. Apply (Iterative closest Point) ICP algorithm to find pose (rotation and translation) of the camera when it captured the depth map in step 5.
9. Use the camera pose information obtained in above step to fuse the TSDF information obtained in step 5  with the global TSDF.
10. Repeat step 6 to step 9 until all depthmaps are fused together.


### Vectorized implementation to extract 2.5D ordered pointcloud from an RGB-D Image
---

Ordered or organized pointclouds contain the information of 3D points arranged in a matrix like structure where the data is split into rows and columns. In case of ordered pointcloud the memory layout of the points is closely related to the spatial layout as represented by these XYZ values which means neighbouring points in the pointcloud are also neighbouring points in the memory layour. This property can be used to speed up the process of calculating normals.
The equation of back-projection is as follows:

<p align="center">
  <img src='/images/math/back-projection.gif' width="100">
</p>
Here K is the 3x3 intrinsic camera matric.

### Stereo Calibration And Rectification

I created a **custom stereo calibration grid using ArUco markers**. This pattern is easier to detect compared to the checkerboard pattern. The marker detection is done using the <a href="https://docs.opencv.org/3.4/d5/dae/tutorial_aruco_detection.html" target="_blank">ArUco class</a> available in OpenCV library. **StereoCam** contains a utility method for capturing and detecting the calibration pattern with just a few lines of code.

<p align="center">
  <img src='/images/calibration.gif'>
</p>
<p align="center">
  Capturing images of the calibration grid for stereo calibration.
</p>

### Generating Disparity Map Using Block Matching Algorithm

The **StereoCam** library has a facility to use the Block Matching algorithm for calculating dense stereo correspondence and the disparity map. The disparity map is then used for the calculation of depth maps. To learn more about disparity map and block matching algorithm please refer to my post <a href="https://learnopencv.com/depth-perception-using-stereo-camera-python-c/" target="_blank">Depth perception using stereo camera</a>. The post also explains how to convert disparity map to depth map. The Block Matching algorithm has several parameters that need to be tuned. **StereoCam** provides an interactive GUI to tune these parameters.

<p align="center">
  <img src='/images/tunedepthParams.gif'>
</p>
<p align="center">
   GIF showing the process of fine-tuning the parameters Using StereoCam GUI.
</p>


### From disparity map to depth map

Disparity and depth are inversely related. An easy way to find an accurate mapping between them is by collecting pairs of depth and disparity values and find the best fitting curve using least-squares fitting.

<p align="center">
  <img src='/images/disparity2depth.gif'>
</p>
<p align="center">
  Capturing depth and disparity data points for curve fitting.
</p>


### Depth Estimation and 2.5 D Projection

Each pixel's depth is estimated using the depth map and the calibrated camera parameters, and a 3D point cloud is generated. This is usually called a 2.5 D projection. **StereoSGBM** method is used to estimate the disparity map, and the **WLS filter** is used to refine the disparity map. The depth map information and intrinsic camera parameters are used to get the 2.5 D representation using **back-projection equation**.

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

A single RGB-D image can be used to generate a 2.5 D pointcloud using back-projection. If we capture RGB-D images of an object from multiple view points, we can reconstruct a 3D model of the object by aligning all the individual 2.5 D pointclouds using methods like ICP - Iterative Closest Point algorithm. 

<p align="center">
  <img src='/images/capturing_stereo.gif'>
</p>
<p align="center">
  Process of capturing stereo images for 3D reconstruction.
</p>

### Generating Anaglyph 3D Movies

We use stereopsis to perceive depth using our binocular vision system. We can simulate such disparities by artificially presenting two different images separately to each eye using a method called stereoscopy. Initially, for 3D movies, people achieved this by encoding each eye’s image using filters of red and cyan colors. They used the red-cyan 3D glasses to ensure that each of the two images reached the intended eye. This created the illusion of depth. The stereoscopic effect generated with this method is called anaglyph 3D.

<p align="center">
  <img src='/images/Anaglyph3D.gif'>
</p>
<p align="center">
  Anaglyph 3D movie created using stereo camera setup
</p>



## Future Work
---

I am working on deep learning-based method for improving the depth estimation results of my stereo camera. It would be interesting to see if a well-designed neural network and a pair of USB cameras can provide high fidelity depth maps comparable to the expensive commercially available RGB-D cameras.


## References
---

I learned the fundamental concepts of stereo vision from <a href="https://www.cse.iitb.ac.in/~sharat/current/cs763/" target="_blank">CS 763 - Computer Vision Course at IIT Bombay (sit through)</a>. This motivated me to develop a custom low-cost stereo camera. Some other references that were useful for me are as follows:

1. <a href="https://docs.opencv.org/master/dd/d53/tutorial_py_depthmap.html" target="_blank">Official OpenCV example - Depth Map from Stereo Images</a>.
2.  C. Loop and Z. Zhang. Computing Rectifying Homographies for Stereo Vision. IEEE Conf. Computer Vision and Pattern Recognition, 1999.
3. Hirschmüller, Heiko (2005). “Accurate and efficient stereo processing by semi-global matching and mutual information”. IEEE Conference on Computer Vision and Pattern Recognition. pp. 807–814.
4. Birchfield and C. Tomasi, “Depth discontinuities by pixel-to-pixelstereo,”International Journal of Computer Vision, vol. 35, no. 3,pp. 269–293, 1999.
5. Similar stereo camera project. <a href="https://github.com/LearnTechWithUs/Stereo-Vision" target="_blank">GitHub link</a>.

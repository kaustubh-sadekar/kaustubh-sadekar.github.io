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
The vectorized implementaiton computes 3D points for each pixel **u** = (x, y) in the incoming depth map **D**<sub>i</sub>(**u**) parallely.
Here K is the 3x3 intrinsic camera matrix. Corresponding 3D points in the camera’s coordinate space are calculated as follows: **vi**(**u**) = **D**<sub>i</sub>(u) **K**−1[u, 1]. This results in a single vertex map (2.5 D pointcloud) **V**<sub>i</sub> computed in parallel. The <a href="https://pytorch.org/docs/stable/generated/torch.meshgrid.html" target="_blank">meshgrid method of PyTorch</a> was used for the GPU accelerated vectorized implementation.

### PyTorch Implementation of Iterative Closest Point (ICP) Algorithm

ICP is commonly used for registration of 3D pointclouds. Kinect Fusion uses **Fast ICP** which combines projective data-association and point to plane ICP algorithm. 




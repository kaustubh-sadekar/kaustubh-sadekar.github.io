---
title: 'Invisibility Cloak'
date: 2019-02-11
permalink: /posts/2019/02/Invisibility-Cloak/
tags:
  - Computer Vision
  - Image Processing
  - Color Spaces
---

## Introduction

The post motivates computer vision enthusiasts to understand the concept of different color spaces. It specifically highlights the effectiveness of HSV colorspace to segment different colors. To make the learning process fun and engaging, I have explained how to create an Invisibility cloak based on the color detection method.

* This post was published on [LearnOpenCV.com](https://learnopencv.com/), a famous website for computer vision. 
* Link to the full blog post is [here](https://www.learnopencv.com/invisibility-cloak-using-color-detection-and-segmentation-with-opencv/).
* Link to the GitHub repository for the project is [here](https://github.com/kaustubh-sadekar/Invisibility_Cloak).

## Project in brief

The following concepts are covered in the post:

1. HSV and RGB color space. 
2. Color detection using HSV color space.
3. Color segmentation using a binary mask.

The key steps involved in creating an invisibility cloak are as follows:
1. Capture a static background image and save it.
2. Use a single color cloth.
3. Set the HSV parameter range according to the color to be detected.
4. Create a segmentation mask for the target color.
5. For each pixel location where the mask is True, replace the pixels of the current frame with the pixel values of the static background image saved in step1.


The following GIF depicts the key steps of the application.

<p align='center'>
  <img src='/images/inv_cloak_process.gif'>
</p>

## Results

<p align='center'>
  <img src='/images/inv_cloak_output.gif'>
</p>

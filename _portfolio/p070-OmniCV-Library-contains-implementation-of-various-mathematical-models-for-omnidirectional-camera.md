---
title: "Study-and-implementation-of-various-omnidirectional-camera-models"
excerpt: "Developed virtual camera classes for various omnidirectional camera models<br/><img src='/images/e2feye.gif'>"
collection: portfolio
---

Project in brief
================

As part of my research related to perception algorithms for omnidirectional cameras, I studied and implemented the following mathematical models for omnidirectional cameras:

* Pinhole Camera Model
* Brown–Conrady model
* Unified Camera Model
* Extended Unified Camera Model
* Kannala-Brandt Camera Model
* Field-of-View Camera Model
* Double Sphere Camera Model

Combining these implementations with functions for interconversion of different representations of 360-degree images, like cubmap, equirectangular, fisheye, and perspective view, I created a library called **OmniCV**.

[**Documentation of OmniCV Library is available here**](https://kaustubh-sadekar.github.io/OmniCV-Lib/index.html)

The idea here was to use the inverse projection function (Unprojection function) to determine the real-world coordinate direction, get corresponding spheric coordinates for a unit sphere and sample the pixel values from a given 360&deg; image (in equirectangular format).

<p align='center'>
  <img src='/images/distCoeff.gif'>
</p>
<p align='center'>
  GUI for pinhole camera with Brown–Conrady model
</p>

<p align='center'>
  <img src='/images/e2feye.gif'>
</p>
<p align='center'>
  GUI for unified camera model
</p>


360° video streaming and viewing GUI for omnidirectional camera
===============================================================

This application is created using the functions provided in the OmniCV library. It is a 360&deg; video player.
Some key points of the application are mentioned below :

* Real-time streaming using an omnidirectional camera.
* Flask server to transform the frame from the stream and provide 360° pan view.
* App interface to view the 360° video with a GUI that enables the user to pan the view.
* Software supports horizontal as well as the vertical orientation of the streaming camera.

<p align='center'>
  <img src='/images/viewer1.gif'>
</p>
<p align='center'>
  Example of 360&deg; viewing GUI
</p>


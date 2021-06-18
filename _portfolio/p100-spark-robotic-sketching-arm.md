---
title: "SPARK a robotic sketching arm"
excerpt: "A 3DOF robotic arm capable of sketching images using ROS and OpenCV <br/><img src='/images/2.gif'>"
collection: portfolio
---

SPARK is a 3DOF ROS based robotic arm capable of sketching any image given to it as input. The robot takes an input image, converts it to binary format, uses the binary image to decide where to draw the pixels, and ultimately creates a sketch of the input image!!


Motivation
==========

I liked sketching in childhood, but I was never so good at it. Although drawing is fun as a hobby, drawing accurate engineering figures using numerous tools was something I never enjoyed. That's when the idea for SPARK came to my mind. I thought of making a robot that could do all these complicated drawings, and I could do some other work.


Project in brief
================

**ROS** based project consisting of three significant nodes.
* Vision node uses **OpenCV** to convert the input image to binary image and states points to be sketched.
* **Inverse kinematics** node to determine angles of the robot joints.
* Embedded node that controls the servo motors to move the robotic arm.

<p align='center'>
  <img src='/images/1.gif'>
</p>
<p align='center'>
    Spark version 1.0 in action
</p>



Update Version 2.0
==================

**Closed-loop control (using only vision-based feedback)** system integrated with ROS
* Vision-based feedback node, using **AruCo marker**, publishes real-time robot joint pose.
* Closed loop control node publishes the motor angles, which are subscribed by the embedded node.

<p align='center'>
  <img src='/images/spark-pid1.gif'>
</p>
<p align='center'>
    Results for P controller
</p>

<p align='center'>
  <img src='/images/2.gif'>
</p>
<p align='center'>
    Results for PD controller
</p>


Update Version 3.0
==================

**OpenAI gym** and ROS compatible robotic arm **simulator for RL and DL based experiments**. 
* OpenAI Gym compatible ROS node for robotic arm simulator.
* **Shadow mode feature** for the simulator to mimic the movements of the actual robot in real-time to get ground truth data and record episodes.
* Draw mode feature to simulate various drawing algorithms.

<p align='center'>
  <img src='/images/3.gif'>
</p>
<p align='center'>
  Shadow mode feature of the simulator
</p>

<p align='center'>
  <img src='/images/spark1.gif'>
</p>
<p align='center'>
  Draw mode feature showing contour based drawing algorithm
</p>


Future Work
===========

* Algorithms to generate **Halftone images** with different pen strokes like points, lines, cross lines, and short random scribbles.
* Developing a model that can learn the inverse kinematics of Spark based on vision-based feedback only.
* Understanding different handwriting styles and developing a conditional GAN that can generate an image containing the content written in target handwriting style.
* Developing a model to enable Spark to learn how to use a calligraphy pen.

{% include comments-providers/disqus.html %}

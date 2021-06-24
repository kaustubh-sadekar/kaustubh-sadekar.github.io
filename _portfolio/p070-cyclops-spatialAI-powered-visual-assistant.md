---
title: "Cyclops - A Spatial AI based Assistant for Visually Impaired People"
excerpt: "<br/><img src='/images/cyclops.gif'>"
collection: portfolio
---

Collaborators: Malav Bateriwala, Vishruth Kumar 

Our project was shortlisted for the <a href="https://opencv.org/announcing-the-opencv-spatial-ai-competition-sponsored-by-intel-phase-1-winners/" target="_blank">OpenCV Spatial-AI competetion sponsored by Intel and OpenCV.org</a>. We were among the **top 32 teams** selected **out of 235** teams all over the world.
We recieved an OAK-D (OpenCV AI Kit with Depth Sensing Capability) to devlop a working prototype of Cyclops.

## Project in brief
---

Cyclops is a spatial AI-based assistant for visually impaired people. It is assembled in a device
similar to a VR headset and allows the wearer to ask if any objects matching the speech input
are found nearby and provides active feedback when approaching found objects.

## Problem description
---
We propose Cyclops as a spatial AI-based assistant for visually impaired people. It is a system assembled in a device similar to a VR headset. The user asks Cyclops to search for a particular object. For example, ’Cyclops, can you see bottle?’ 
Cyclops searches for the object and guides the visually impaired user using active audio feedback. The overall problem statement consists of two main stages:

1. **Determining the object to be searched based on the user input**

This can be done using NLP based speech recognition methods. The input is provided in
the form of a speech signal by the user, which is converted to a text signal using speech
recognition methods. The generated text signal is used to determine the object to be
searched.

2. **Guiding the user to the target object**

Object detection can be performed using the OAK-D neural inference functionality. If the object to be searched is detected, audio feedback will be generated, along with additional
information about its location with respect to the user. e.g., ”Object at 3 meters in the right” or ”Object is far away in the left.” The approximate distance of the target object
will be calculated using the depth functionality of OAK-D. The audio feedback will be generated continuously based on the updated values from OAK-D until the user reaches the object.

Finally, the user will be informed when the object is close enough to be held.





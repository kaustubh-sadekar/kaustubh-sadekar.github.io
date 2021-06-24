---
title: "Cyclops - A Spatial AI based Assistant for Visually Impaired People"
excerpt: "<br/><img src='/images/cyclops.gif'>"
collection: portfolio
---

Collaborators: **Malav Bateriwala, Vishruth Kumar**

Our project was shortlisted for the <a href="https://opencv.org/announcing-the-opencv-spatial-ai-competition-sponsored-by-intel-phase-1-winners/" target="_blank">OpenCV Spatial-AI competetion sponsored by Intel and OpenCV.org</a>. We were among the **top 32 teams** selected **out of 235** teams all over the world.

<p align='center'>
  <img src='https://opencv.org/wp-content/uploads/2020/07/OpenCV-Spatial-AI-Competition-Phase-1.png' width="300">
</p>

We received an OAK-D (OpenCV AI Kit with Depth-Sensing Capability) to develop a working prototype of Cyclops.

## Project in brief
---

Cyclops is a spatial AI-based assistant for visually impaired people. It is assembled in a device
similar to a VR headset and allows the wearer to ask if any objects matching the speech input
are found nearby and provides active feedback when approaching found objects.

## Problem description
---
We propose Cyclops as a spatial AI-based assistant for visually impaired people. It is a system assembled in a device similar to a VR headset. The user asks Cyclops to search for a particular object. For example, ’ Cyclops, can you see bottle?’ 
Cyclops searches for the object and guides the visually impaired user using active audio feedback. The overall problem statement consists of two main stages:

1. **Determining the object to be searched based on the user input**

This can be done using NLP based speech recognition methods. The input is provided in
the form of a speech signal by the user, which is converted to a text signal using speech
recognition methods. The generated text signal is used to determine the object to be
searched.

2. **Guiding the user to the target object**

Object detection can be performed using the OAK-D neural inference functionality. If the object is detected, audio feedback will be generated, along with additional
information about its location with respect to the user. e.g., ”Object at 3 meters in the right” or ”Object is far away in the left.” The approximate distance of the target object
will be calculated using the depth functionality of OAK-D. The audio feedback will be generated continuously based on the updated values from OAK-D until the user reaches the object.

Finally, the user will be informed when the object is close enough to be held.

<p align='center'>
  <img src='/images/cyclops-illustration.png' width=400>
</p>
<p align='center'>
    Illustration of a scenario where Cyclops guides the user to pick up a water bottle. 
</p>


<p align='center'>
  <img src='/images/cyclops-product.png' width=400>
</p>
<p align='center'>
    Product sketch for Cyclops. 
</p>


## Implementation Details
---

Cyclops is developed using **OAK-D** and **Raspberry-Pi3**, and the code is developed in python. Processing of the code is divided into two threads using multi-threading. One thread takes care of taking audio input from the user and generating audio feedback. The second thread deals with the acquisition of the data from the OAK-D device.

###  Speech To Text Conversion

<a href="https://github.com/Uberi/speech_recognition#readme" target="_blank">Speech recognition library</a> is used to take user input as an audio signal and convert it to text. Several other speech-to-text libraries were explored, but we used this library because it supports several speech-recognition APIs and engines online and offline. For our experiments, we use the method that supports <a href="https://cloud.google.com/speech-to-text" target="_blank">Google Cloud SpeechAPI</a> to convert audio signal to text. 

The generated text is then used to determine the object to be searched. We make use of the identification phrase - **” Cyclops can you see. ”** The word after this phrase is stored as the target object to be searched. Hence to search for a bottle, the user needs to say - ” Cyclops can you see bottle”.


###  Object Detection
Object detection is performed by the OAK-D device connected to the host processor- Raspberry Pi 3B. The <a href="https://github.com/luxonis/depthai" target="_blank">depthai API</a> was used to obtain the image data captured by the OAK-D camera as well as the predictions. We use the **MobileNetSSD** model for object detection. OAK-D also provides depth information which can be combined with the object detection model to determine the distance of the detected objects from the camera. 

<p align='center'>
  <img src='https://github.com/kaustubh-sadekar/OAK-D_Experiments/blob/master/exp/color_disparity.gif' width=400>
</p>
<p align='center'>
    Depthmap returned by the OAK-D. 
</p>


### Audio Feedback

Once the target object is searched, audio feedback must be generated to guide the user if Cyclops detects the target object. 

The output of object detection and the respective distance information is stored in global variables to be accessed by both the vision thread and the audio thread. Based on this information, the audio output is generated to guide the user. For example, if the object is detected and 2 meters away in front of the user, the audio feedback would say, ”Object at 2 meters in front”. This way, the users can orient themselves and move towards the target object.

**Google Text-To-Speech (gTTS)** library is used to generate audio files from strings, and **mpg123 API** is used to play the generated audio files.


Project demo video is available <a href="https://www.youtube.com/watch?v=OEDmPRZ-ZPM" target="_blank">here</a>.

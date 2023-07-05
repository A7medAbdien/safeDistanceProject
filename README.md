﻿Here I will be covering the theory part of both apps, my thoughts, and maybe a touch of my struggle and complains.

# Problem statement

https://github.com/A7medAbdien/safeDistanceDoc/assets/102335234/cbe7e688-d257-4b8e-abcd-176b4a067cc8

The key challenge in combating the COVID-19 virus is to prevent its transmission. One effective way to do this is by maintaining a safe distance between individuals. However, accurately predicting distance can be difficult, especially for children. This is where an application that can help measure the distance between people can be useful. 

**Notice** that I measure the distance between the camera and a person, not a person and another. 

I suggested two solutions to measure the distance:
1. Android based
2. web-based (using flask framework).


# High-Fidelity Prototype

Sure there where a Low-Fidelity but I will keep for myself 😊. 

![High-Fidelity Prototype]()
 

# UML Use case diagram

UML Use Cases are visual representations of the usage requirements for a system. They are detailed and specific, and are used during actual development to provide a clear and comprehensive description of the system's requirements. UML Use Cases provide an actionable description of the core functionality of the system.

![Use case diagram]()


The project use case diagram consists of two main activities:

**Start Camera:** The user starts the camera device to measure the distance between the detected object and the camera. The user can show lines between person poses and the current distance between the detected person on this person. The user can show the detection information, like frame rate, image size, and detection latency. If the current distance between the detected person and the camera is less than the safe distance, an alert will be shown.

**Change Settings:** The user can hide detection information, lines between person poses, and the current distance that is rendered on the detected person. 


# UML Sequence diagram

UML Sequence diagrams are a type of diagram used to describe interactions among classes in terms of an exchange of messages over time. They are also known as event diagrams. Sequence diagrams are useful for visualizing and validating various runtime scenarios, as they provide a clear and detailed representation of the interactions and messages that take place between different classes in a system.

![Sequence diagram]()

**Step 1:** The user touches the screen to start the application.
**Step 2:** The application system opens the device camera.
**Step 3:** The camera gets live streaming data and processes them to match the detector input.
**Step 4:** If there is a detected object, the renderer gets created and receives the orientations of the detected object. 
**Step 5:** The renderer calculates the distance between the detected object and the camera device.
**Step 6:** In case the calculated distance (in step 5) is less than the safe distance, the renderer will send an alert to the application system.
**Step 7:** the application system shows the user an alert message.
**Step 8:** If there is a detected object, The renderer draws lines on the detected person.


# Implementations

So, after searching and digging I found those solutions.

![Implementations table]()

Consider the main keywords I used were: Augmented Reality, Body detection, Pose detection. Then I realized that Machine Version would be more accurate than Augmented Reality.

I thought about using face detection build in ARCore and measure the distance from the face to the camera, but ARCore detect faces only in one meter (at this point of time). 


# Distance measurement

We measure the distance based on 2D understanding. We build an understanding of the actual scene based on the distance between two points in an image. For example, In the figure below shows the railroad ties appear to get smaller as they get further away from us. If we measured the apparent size of each railroad tie, their measured size would decrease in proportion to their distance from our eyes (Brothaler, 2013). The same thing happens to the distance between shoulders and hips. 
 
![Represent distance in flat images]()

In order to calculate the distance between the camera and the detected person, we calculate the distance between the shoulders and hips. We could use the pupillary distance (PD), the distance between the centers of your pupils since it is fixed as the detected person moves, but it will be affected if the detected person did not face the camera. We used the distance between the shoulders and hips, considering if the detected person rotated.

$X_{landmark size}=  \frac{RightSideDistance_{Shoulder→Hip} + LeftSideDistance_{(Shoulder→Hip)}} {2}$

We get the average of the distance right side and the left one, then using a polynomial equation we get how far the detected person is from the camera. The coefficients of the polynomial equation calculate by measuring the size of landmarks, the distance between shoulders and hips, to how far a person is from the device camera.

# Summery

I developed two solutions to measure the distance, but both based on a 2D understanding, and to go for measuring based on 3D understanding we would use ARCore, but the tool was decrypted at this point of time (07/2022->02/20223) under the promise of development.
 
Alternatively, It is easy to be done in Apple ecosystem or this should not be your first experience application development. I did all of this in two months, learning, planing and developing, while the solution in [Apple is in 15min](https://youtu.be/JAdKXV2y_Js). So ya I like Google but I love Apple.
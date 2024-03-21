# Using-a-3D-camera-to-identify-objects-and-their-positions-in-the-industrial-robot-environment.


# Description:
This repository contains the implementation of an algorithm developed to identify objects within the operational area of an industrial robot using a 3D camera manufactured by Cognex. The project aims to streamline object recognition processes in industrial settings, enhancing automation and efficiency.
# Scope of Work:
Acquiring knowledge about the Cognex 3D camera and Cognex Designer software.
Understanding fundamental concepts of vision systems and various types of vision systems.
Setting up a research platform and configuring the camera with a laptop.
Testing software functionalities and optimizing image quality by adjusting exposure time.
Creating and testing object identification references, involving multiple image registrations and object identifications.
Assigning coordinates to language data and developing an HMI panel in Cognex Designer for visualizing 3D images and detected objects, enabling reference selection and displaying coordinates of each object.
Configuring communication between the camera and industrial robot, transmitting object coordinate data via TCP/IP protocol.
# Usage:
To utilize the algorithm for object identification in your industrial robotics projects, follow the instructions provided in the documentation and codebase.

# Vision Pro
So, let me tell you about VisionPro. It's this top-notch computer vision tool from Cognex that's just fantastic. I mean, it's like your go-to software for setting up and running vision applications with any camera you've got. With VisionPro, you can do a whole bunch of stuff – measuring, positioning, checking, identifying, aligning, you name it. And it's not just basic stuff; it's got all these cool features for inspecting SMT electronic components after they've been placed. 

But here's the best part: VisionPro comes packed with this huge library of functions. Seriously, it's like a treasure trove for anyone working with vision systems. You've got tools for matching points, lining things up, filtering images, recognizing patterns, reading barcodes – you name it, it's probably in there. And hey, it plays nice with NET, C#, and C++ libraries, so you've got plenty of options for integrating it into your projects.

# Measurements
![Dlugosc50mmPokazaneJak](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/be5e7897-a8f3-436a-abbf-132c6bed4e58)
![Wysokosc15,3mm_szerokosc30mm](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/17e68c37-b550-4054-87c8-ec7bc9c9a663)


# Block diagram 
![Schemat_blokowy](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/3bb5d5ff-29a0-47a8-96e8-8e97abc6a019)
Program block diagram

![Blok_VisionPro](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/dd9d4353-f428-4ca4-bdbe-286390c25106)
3D camera block on block diagram 

# Identification of a pattern and training of functions for its recognition.
![Idne](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/32f31816-7dc3-410e-8dd5-c4bba2bfadf4)


# Detecting a pattern in an image 
![Wykryty_obj](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/0009a521-964a-4008-b15b-6949c65145d7)

In the camera's workspace, each element has coordinates. Trained functions recognize matching objects based on pattern identification, with their coordinates declared in the Tag manager for display on the control panel and transmission to the industrial robot. It's crucial to determine the maximum number of objects in the camera's field of view before capturing a photo. If we define fewer variables than the actual objects, surplus ones will be ignored. In our case, we assumed a maximum of five elements of one type.

# Camera-Robot Communication - Algorithm of program operation.
![Algorytm](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/b2b6fd43-00c3-42ab-8b7d-cd8617c7202c)

After launching the program, a reference is selected for the object identified based on the camera image in the subsequent step of the algorithm. Following this operation, image analysis takes place, and the coordinate data is read, which is then transmitted to the robot via the TCP/IP protocol (handled by a server launched using the Cognex Designer environment). Based on the object's coordinate data, the robot retrieves it from the production line and moves it to another location. If the robot performs the operation correctly, the next object is identified, and the cycle repeats again.

# Program code for handling camera-robot communication. 
![Code1](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/10c5742c-5b21-466a-9d48-680e79a0cbfc)

The server program code has been presented, which is executed upon data appearing on the communication socket of the TCP/IP protocol. There is a code snippet responsible for the camera's login to the TCP/IP server. Logging in the camera application to the server is required using the default username "admin" and password "1234". The camera application connects to the server in client mode. After logging into the server, the application sends it the first command named "GO", which is responsible for sending a request to the robot to enter asynchronous listening mode (this does not block commands for the robot from the parent system). The next command sent to the server is "GF", which causes a test command to be sent to the robot. The command "GVE076" is responsible for sending the number of identified objects to the robot. The number, 38, is transmitted in floating-point format, with precision set to 3 decimal places.

![Code2](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/31be47a9-9de4-4354-af7c-5d80913cb1a7)
In the next section of the code, data transmission about the coordinates of objects occurs. The X-axis coordinate is preceded by the command GVJ096, the Y-axis by the command GVK096, and the Z-axis by the command GVL096.

![Code3](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/3119dc97-b1cb-480b-bc00-fae482b37dc1)
In the final image, a snippet of the program code responsible for transmitting data to the robot regarding the coordinates of objects identified by the camera is presented. All coordinate data is transmitted rounded to 3 decimal places.

# Test station for a 3D camera.
![Station_test](https://github.com/KamilGodek/Wykrywanie_twarzy/assets/135075598/53ccf14f-db09-493f-9f00-cea796cd891f)

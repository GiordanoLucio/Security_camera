# Security_camera
Project done in collaboration with Carmine D'Alessandro for the exam 'Serverless Computing'.<br><br>
The goal of the project was to develop an application for the communication of one or more sensors with a broker of message through the MQTT protocol, the analysis of data and the notification of the end-user if a particular condition was met.

## Overall architecture
The project can be divided into 3 main parts:
* Sending client
* Message broker
* Receiving client

A sending client (CameraClient) sends frames to topics of the broker in specific time slots. On the other side, two receving clients (MonitorClient, SecurityClient) subscribe to those topics and receive the information. The SecurityClient analyzes the image and detects the number of faces. This information is send back to the message broker which triggers the [IFTTT](https://ifttt.com/) API and send a notification to the Telegram chat of the user.
### Camera client
The Camera client is made in Python 3.6 and uses the library [pika](https://pypi.org/project/pika/) to establish a connection with the message broker. <br>
The client uses the camera of the computer to register the video stream and, from all the frames deteced in the stream, send some frames to the message broker in two different topics, "view" and "security", according to the future use. The number of frames sent in the two streams is different.
### Message broker
The technologies involved to develop the message broker are Nuclio, RabbitMQ and Docker in a Linux environment.<br>
We used Docker as a container, and Nuclio and RabbitMQ to set up the infrastructure to manage the MQTT messages.<br>
The message broker also is responsible for the communication between the Security client and the IFTTT WebHooks.
### Monitor client
The monitor client subscribes to the topic "view" using pika and shows the corresponding frames in a video.
### Security client
The security client subscribes to the topic "security" using pika and analyzes the frames to recognize the number of faces present in the image. Each time it recognize any face, it sends a message to the broker which triggers the IFTTT WebHooks API.
### IFTTT WebHooks
To make this part to work, it is needed to create an applet. Log in with your account and select "New Applet". Select as "this" condition the WebHooks and as "that" condition the one you prefere. We used a Telegram chat notification.

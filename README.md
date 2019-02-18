# Security_camera
This project was done in collaboration with Carmine D'Alessandro for the exam 'Serverless Computing'

Structure:
We create a serverless architecture using docker and rabbitmq as a broker. After some logic, we send a message to nuclio from a security client, where we trigger an ifttt message.
Camera client: takes 3.x frames per second from the camera and send them to the rabbit exchange.
Monitor: reads the frame sent by the camera client
Security: when an event (mocked up by the camera client) occurs, reads the frame and checks if there are faces in it.

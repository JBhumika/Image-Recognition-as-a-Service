# AWS Image Recognition as a Service

## Overview

The AWS Image Recognition Service is a cloud application that provides image recognition as a service to users by utilizing AWS cloud resources to perform deep learning on images provided by the users. The application handles multiple requests concurrently and automatically scales out and in based on demand. The project utilizes the following AWS services:

* Elastic Compute Cloud (EC2)  
* Simple Queue Service (SQS) 
* Simple Storage Service (S3)

The deep learning model used for image recognition is provided in an AWS image (ID: ami-07303b67, Name: imageRecognition, Region: us-west-1a).

### Web-Tier-AWS
The Web-Tier-AWS is a RESTful web service that accepts user requests in the form of an image URL and puts the request body onto an input queue. It then starts listening to the output queue for the response. This application also includes a load balancer service that creates app instances when the request demand increases (scale out).

### Listener
The Listener application runs inside the app instances and listens for messages (requests) in the input queue. When a message arrives, it runs the deep learning model for classification and puts the classification result into an S3 bucket. The classification result is also inserted into the output queue. When there are no messages in the input queue, the application shuts down the instance in which it's running, facilitating scale-in.

### Listener Running
The Listener Running application is the same as the Listener application, but the instance running this application won't terminate at all, facilitating quick response to the user.

### Usage
To use the application, send HTTP requests in the following format:
```
http://[IP Address]/cloudimagerecognition?input=[URL of the image]
```
For Example,
```
example: http://13.57.186.192:8080/cloudimagerecognition?input=http://visa.lab.asu.edu/cifar-10/104_automobile.png
```

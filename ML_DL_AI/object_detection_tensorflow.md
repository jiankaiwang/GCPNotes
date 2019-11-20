# Creating an Object Detection Application Using TensorFlow (GSP141)



The tutorial demonstrates how to establish a web app of object detection task. The concept is:
* Train an object detection model or fetch a pre-train model.
* Establish a web app allowing a request for object detection using the above model.

In this tutorial, the app is more like a webpage for interaction instead of a service API. The model part is using a pre-trained model downloaded from an object detection zoo.



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/1823?parent=catalog
* Git Repo:
  * Webpage Service: https://github.com/GoogleCloudPlatform/tensorflow-object-detection-example
  * Object Detection: https://github.com/tensorflow/models/tree/master/research/object_detection
  * Object Detection Model Zoo: https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md



## Quick Notes



### Object Detection API

The Object Detection API is built on the top of Tensorflow. The API tries to specify the workflow of training an object detection model. The basic workflow is:
* Prepare datasets in `tfrecord` format.
* Customize the training pipeline.
* [Optional] From a fine-tune model or a pre-trained model.
* Begin training the model and it will auto export the trained model.

The more details refer to https://github.com/jiankaiwang/TF_ObjectDetection_Flow.






















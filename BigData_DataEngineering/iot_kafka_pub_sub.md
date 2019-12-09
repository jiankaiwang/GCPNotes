

# Streaming IoT Kafka to Google Cloud Pub/Sub (GSP285)



In this tutorial, demonstrate combining both Cloud Pub/Sub and Kafka services to interchange messages. The demo maybe not the best solution but as a demo for integration flexibility. For example, you are considering using the event notifications from Cloud Pub/Sub to the streaming data.

keywords: `Kafka`, `Cloud IoT Core`


## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/2766?parent=catalog


## Quick Note

The core of Cloud Pub/Sub and Kafka is similar. Each of them is consisted of two main components, publisher(or producer) or subscriber(or consumer). First, you have to create a topic in either Cloud Pub/Sub or Kafka. A service playing as a publisher publishes a message to a topic and other services as a subscriber consumes the message from the topic.

In the tutorial, Cloud Pub/Sub plays the opposite rule against Kafka. The publisher of one side is linked to the consumer of the opposite side and vice versa.

Due to different protocols between two services, a connector was created to link two sides' topics, for example, `from-kafka` and `to-pubsub` or `to-kafka` and `from-pubsub`.


![](https://cdn.qwiklabs.com/9mJIpleWQZZdy%2FyobF2CWRBB3X1eILcKyJdUFt%2FquBE%3D)

Such architecture supports streaming data and is also extendable, liking adding Cloud IoT Core. The Cloud IoT Core sends the message to the Cloud Pub/Sub and then trigger the flow interchanging message with Kafka. The higher consumer on the Kafka deals with the messages. In addition, send the handling status back to the Cloud Pub/Sub service.

![](https://cdn.qwiklabs.com/a7O9v0GnbuRcg0d2UIIyKsbDiGVLw6GgNFiMZTw4F80%3D)
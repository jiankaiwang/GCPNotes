# Google Cloud Platform Reference



## Notes



### API Documents

*   [BigQuery](api_doc/bq_api.md) (bq)
    *   BigQuery Machine Learning (`BQML`)
*   [Storage](api_doc/gsutil_api.md) (gsutil)
*   [Google Cloud](api_doc/gcloud_api.md) (gcloud)
    *   Project (`gcloud projects`): config settings
    *   Compute Engine (`gcloud compute`): firewall rules, backend services
    *   Pub/Sub (`gcloud pubsub`)
    *   Cloud function (`gcloud function`)
    *   Container (`gcloud container`): K8s clsuters
    *   IAM (`gcloud iam`)
    *   IoT (`gcloud beta iot`)
*   [Vision](api_doc/gvision_api.md) (Vision Annotation)
    *   Restful API
*   [Speech](api_doc/gspeech_api.md) (Speech to Text)
    *   Restful API
*   [Translation](api_doc/gtranslation_api.md) (Translation from one language to another)
    *   Restful API
*   [Natural Language](api_doc/gnl_api.md) (Classify text into categories)
    *   Restful API
*   [ML Engine](api_doc/gml_engine.md) (a.k.a. `gcloud ai-platform `.)
*   [Video Intelligence](api_doc/gvideo_API.md)
    *   Restful API
*   [Datalab](api_doc/datalab_api.md) (datalab)



## Topics

### Machine Learning / Deep Learning / Artificial Intelligence

A workflow shows how to do an End-to-End ML or AI works on the Google Cloud Platform.

![](https://cloud.google.com/images/ai-platform/cloud-ai-platform.svg?hl=zh-tw)



* [Introduction to APIs in Google](ML_DL_AI/google_api.md) (GSP294)
    * Definition of (RESTful) `APIs` and `Endpoints`
    * Introduction to `Google APIs`
*   [Predict Taxi Fare with a BigQuery ML Forecasting Model](ML_DL_AI/Predict_Taxi_Fare.md) (GSP246)
    *   Train and evaluate a linear model via `BQML` on a public `BigQuery` dataset.
    *   Use a `BQML` built-in `regression` model.
*   [Predict Visitor Purchases with a Classification Model in BQML](ML_DL_AI/Predict_Visitor_Purchases.md) (GSP229)
    *   Train and evaluate a logistic regression model via `BQML` on a public `BigQuery` dataset.
    *   Use a `BQML` built-in `classification` model.
* [Detect Labels, Faces, and Landmarks in Images with the Cloud Vision API](ML_DL_AI/Cloud_Vision_API.md) (GSP037)
    *   Use `curl` command to send a JSON request to `Cloud Vision API` and parse its response.
    *   Use an image from a `cloud storage bucket` as the data source for a `Cloud Vision API` request.
* **[Awwvision: Cloud Vision API from a Kubernetes Cluster](ML_DL_AI/k8s_cluster_vision_api.md) (GSP066)** :star:
  
    *   Deploy and run multiple services on several `containers` of a `Kubernetes cluster` on a time.
    * Create a worker for a web crawler and requests for `Vision API`, a web-app for viewing results and a cached database.
* [Google Cloud Speech API: Qwik Start](ML_DL_AI/Cloud_Speech.md) (GSP119)
  
    *   Use a `curl` command to request the `Speech API` for speech to text transformation.
*   [Speech to Text Transcription with the Cloud Speech API](ML_DL_AI/Cloud_Speech_2.md) (GSP048)
    *   Ask a Speech-to-Text translation request in a `restful API` (`Speech API`) format.
    * Ask for doing translation over multiple languages.
* [Translate Text with the Cloud Translation API](ML_DL_AI/Cloud_Translation.md) (GSP049)
  
    *   Request restful `Translation API` calls for the language translation to a target language, or for detecting which the language of the sent text is.
* **[Classify Text into Categories with the Natural Language API](ML_DL_AI/Natural_Language.md) (GSP063)** :star:
  
    *   Request a `Natural Language API` call for a categorical analysis on a text dataset (from BBC). 
    * Save the result from requests into a `BigQuery` table and analyze it on `BigQuery`.
    * Create a service account via `Cloud IAM` to allow python scripts to request `Natural Language API` and access to `BigQuery`.
* [Entity and Sentiment Analysis with the Natural Language API](ML_DL_AI/entity_sentiment_nl.md) (GSP038)
  
    *   Create a request and send a sentence to `Natural language API` in order to do the `entity analysis`, `sentiment analysis`, `entity-sentiment analysis`, `syntax analysis` and `linguistic analysis`.
*   **[Extract, Analyze, and Translate Text from Images with the Cloud ML APIs](ML_DL_AI/Cloud_ML.md) (GSP075)** :star:
* Request a number of `machine learning restful APIs` orderly. 
    * Firstly request a `Vision API` call for text detection with OCR to an image, secondly request a `Translation API` call for detecting language and translating it from the text, thirdly request a `Natural Language API` call for the entity analysis from the translation result.
*   **[Scanning User-generated Content Using the Cloud Video Intelligence and Cloud Vision APIs](ML_DL_AI/Cloud_Video_Vision.md) (GSP138)** :star:
    * An example scenario firstly an image is uploaded to `cloud storage bucket` triggering a notification to `Cloud Pub/Sub` that then triggers cloud function, secondly cloud functions would request both `Vision API` and `Video Intelligence API` calls, and after the responses from APIs are received the result would be written into `BigQery` table. In the final, you can analyze the data in the `BigQuery`.
* [Classify Images of Clouds in the Cloud with AutoML Vision](ML_DL_AI/classify_image_automl_vision.md) (GSP223)
    * How to use `AutoML` to train and evaluate a model on the data hosted on `Cloud Storage`.
    * How to deploy the model trained over `AutoML`.
* [Implementing an AI Chatbot with Dialogflow](ML_DL_AI/ai_chatbot_dialogflow.md) (GSP078)
    * How to use `Dialogflow` to build a `chatbot`.
    * How to deploy or integrate the `chatbot` with your own service.
* **[Machine Learning with TensorFlow](ML_DL_AI/ML_Tensorflow.md) (GSP273)** :star:
    * Introduce using Tensorflow `estimator API` to build a linear classifier from scratch.
    * Introduce how to submit a job to the Google `Cloud AI platform`.
* [TensorFlow for Poets](ML_DL_AI/tf_poets.md) (GSP077)
  * Introduce how to use `transfer learning` to speed up a image classification retraining task.
* [Creating an Object Detection Application Using TensorFlow](ML_DL_AI/object_detection_tensorflow.md) (GSP141)
    * Train an `object detection` model or fetch a pre-train model via `object detection API`
    * Establish a `web app` allowing a request for object detection using the above model.
*   **[AI Platform: Qwik Start](ML_DL_AI/Cloud_ML_Engine.md) (GSP076)** :star:
  *   Use a structured dataset as an example to demonstrate a complete flow of machine learning on the local and furtherly submit the job to the `AI platform`.
* [Predict Housing Prices with Tensorflow and AI Platform](ML_DL_AI/housing_prices_tf_ai_platform.md) (GSP418)
    * Perform how to start a `Datalab` and run a Python notebook on it.
* [Image Classification of Coastline Images Using TensorFlow on AI Platform](ML_DL_AI/img_cls_tf_aiplatform.md) (GSP014)
    *   Perform how to start a `Datalab` and run a Python notebook to do a task of image classification.



### Big Data / Data Engineering

```text
Dataprep: Data Transformation Pipeline
   ｜
Dataflow: Batch or Streaming Data Processing Pipeline
   ｜
Dataproc: Hadoop or Spark Computing Core
```

*   [Dataprep: Qwik Start](BigData_DataEngineering/Data_Prep.md) (GSP105)
*   [Dataprep: Creating a Data Transformation Pipeline with Cloud Dataprep](BigData_DataEngineering/Data_Prep_Pipeline.md) (GSP430)
*   [Dataflow: Qwik Start - Templates](BigData_DataEngineering/Data_Flow_Templates.md) (GSP192)
*   [Dataflow: Qwik Start - Python](BigData_DataEngineering/Data_Flow_Python.md) (GSP207)
*   [Dataflow: Run a Big Data Text Processing Pipeline in Cloud Dataflow](BigData_DataEngineering/Data_Flow_Pipeline.md) (GSP047)
*   [Dataproc: Qwik Start - Console](BigData_DataEngineering/data_proc_console.md) (GSP103)
*   [Dataproc: Qwik Start - Command Line](BigData_DataEngineering/data_proc_cli.md) (GSP104)
*   [Cloud IoT Core: Building an IoT Analytics Pipeline on Google Cloud Platform](BigData_DataEngineering/cloud_iot_core.md) (GSP088)
*   [Cloud Pub/Sub: Streaming IoT Kafka to Google Cloud Pub/Sub](BigData_DataEngineering/iot_kafka_pub_sub.md) (GSP285)
*   [ETL Processing on GCP Using Dataflow and BigQuery](BigData_DataEngineering/etl_gcp_dataflow_bigquery.md) (GSP290)



### Cloud Infrastructure

The following courses are mainly related to `GCP Essentials` on qwiklabs.

*   [A Tour of Qwiklabs and the Google Cloud Platform](CloudInfrastructure/qwiklab_gcp.md) (GSP282)
*   [Creating a Virtual Machine](CloudInfrastructure/Create_VMs.md) (GSP001)
*   [Compute Engine: Qwik Start - Windows](CloudInfrastructure/Create_Windows_VMs.md) (GSP093)
*   [Getting Started with Cloud Shell & gcloud](CloudInfrastructure/cloud_shell_gcloud.md) (GSP002)
*   [Kubernetes Engine: Qwik Start](CloudInfrastructure/gke_start.md) (GSP100)
*   [Set Up Network and HTTP Load Balancers](CloudInfrastructure/network_http_balancer.md) (GSP007)














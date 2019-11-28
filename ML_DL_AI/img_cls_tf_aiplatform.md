# Image Classification of Coastline Images Using TensorFlow on AI Platform (GSP014)



This tutorial shows how to start a Datalab instance. A Datalab instance is like a Jupyter notebook but hosted on the cloud with lots of built-in packages. You can run a complete model training and inference workflow from scratch on it. Unlike the other local task, the tutorial uses lots of cloud services, like Storage buckets, BigQquery, and AI Platform. 



keywords: `Datalab`



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/1048?parent=catalog



## Quick Notes



### About the packages

In the tutorial, there are two mainly used packages running in the python environment, that are `ML Toolbox` and `google.datalab`.

Package `google.datalab` helps you process data and connection with GCP easier. Some useful APIs are listed.
* google.datalab.bigquery: helps to operate BigQuery
* google.datalab.ml: helps to initialize a CloudML Job, starts a TensorBoard for monitoring training progress, etc.
* google.datalab.stackdriver: monitors the log
* google.datalab.storage
* ...

Package `ML Toolbox` helps you quickly establish a model whatever it is a classification, regression or image classification task. Soma useful APIs are listed.
* mltoolbox.classification: includes linear or dnn methods
* mltoolbox.regression
* mltoolbox.image.classification
* ...

More details about the above packages refer to webpage (https://googledatalab.github.io/pydatalab/google.datalab.ml.html).

If you want to build a local datalab-enabled environment, you can check the git repo (https://github.com/googledatalab/pydatalab).





### Datalab on the Local

**It's much easier running a Datalab instance on GCP instead of a local environment.** 

The following commands help you create a local Datalab environment.
```sh
sudo apt-get update
sudo apt-get install -y python-pip python-dev python3-pip python3-dev
pip install virtualenv

virtualenv -p python3 datalab
source ./datalab/bin/activate
pip install datalab jupyter
./datalab/bin/jupyter nbextension install --py datalab.notebook --sys-prefix
```

More details refer to webpage (https://github.com/googledatalab/pydatalab).
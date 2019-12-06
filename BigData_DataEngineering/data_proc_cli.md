# Dataproc: Qwik Start - Command Line (GSP104)


Cloud Dataproc is a fast, easy-to-use, fully-managed cloud service for running Apache Spark, Apache Hadoop clusters. It is convenient for customers to create a Google Cloud Dataproc cluster, resize or modify the number of worker in it.

keywords: `Dataproc`


## Reference

* Qwiklabs: <https://google.qwiklabs.com/focuses/585?parent=catalog>



## Quick Note



### Create a cluster

Run the following command to create a cluster called `example-cluster`.

```sh
gcloud dataproc clusters create example-cluster
```

You will be asked to choose a zone for your new created cluster. You can press `Enter`.



### Submit a job

Run the following command to submit a Spark job that calculates a rough value for pi.

```sh
gcloud dataproc jobs submit spark --cluster example-cluster \
  --class org.apache.spark.examples.SparkPi \
  --jars file:///usr/lib/spark/examples/jars/spark-examples.jar -- 1000
```

**Parameters passed to the job must following a double dash (--).**

You should see the similar result like `Pi is roughly 3.1418577514185775` in the console after the job was complete.



### Update a cluster

Run the following command to change the number of workers in the cluster.

```sh
gcloud dataproc clusters update example-cluster --num-workers 4
```





